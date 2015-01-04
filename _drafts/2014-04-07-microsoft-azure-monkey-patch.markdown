---
layout: post
title:  "Microsoft Azure Ruby API Monkey patches"
date:   2014-04-07 20:06:38
categories: cloud microsoft azure 
---
 Microsoft's cloud offering is called <strong>Azure</strong>.

I was working with the azure ruby gem and found it lacking in a couple of respects.

<ul class="list-group">
  <li class="list-group-item">When an image is retrieved any media link info is simply ignored</li>
  <li class="list-group-item">There is no interface for retrieving storage account keys</li>
</ul>

This is a pain.  Media link info is important for user created images.  From this
string you can pull info regarding the virtual hard disk, including the storage account name.
You will need the storage account name if you plan on creating virtual machines from
your own images.

Now, let's say you create a virtual machine from that nifty image of yours.  This 
will create a virtual machine that is associated with a virtual disk that is associated
with a vhd file.  When you destroy the virtual machine via the azure gem's
delete\_virtual\_machine method both the virtual machine and it's disk are destroyed,
but that 30 gig vhd file sticks around.  

It would be nice if there was an option
 on the delete\_virtual\_machine method to delete the vhd as well, but no such luck. 
So you need to take an extra step to delete the vhd file.

This is accomplished via the BlobService delete\_blob command with code that looks
something like:

{% highlight ruby %}
  Azure.configure do |config|
    config.storage_account_name = <storage_account_name> 
    config.storage_access_key   = <storage_access_key> 
  end
  blobservice = Azure::Blob::BlobService.new
  blobservice.delete_blob('vhds', vhd_name) # container, blob_name
{% endhighlight %}

The example above shows that the storage account key is needed by the azure
gem to destroy vhd (via the delete\_blob method) 

So, if I want to create and delete virtual machines without having to hardcode
in my storage account names and keys then I either need to interface directly
with the Azure REST API, or I need to monkey patch the azure ruby gem.  I decided
on the monkey patch.  If you are using the azure gem then you should be able to drop
this code into your project and have it work.

{% highlight ruby %}
 ## Monkey patches for the Azure gem ##

 # return the medialink value when fetching virtual machine images
 Azure::VirtualMachineImageManagement::Serialization.class_eval do
   def self.virtual_machine_images_from_xml(imageXML)
     os_images = Array.new
     virtual_machine_images = imageXML.css('Images OSImage')
     virtual_machine_images.each do |image_node|
       image = VirtualMachineImage.new
       image.os_type = xml_content(image_node, 'OS')
       image.name = xml_content(image_node, 'Name')
       image.category = xml_content(image_node, 'Category')
       image.locations = xml_content(image_node, 'Location')
       image.instance_eval do
         @media_link = xml_content(image_node, 'MediaLink')
         def media_link
           @media_link
         end
       end
       os_images << image
     end
     os_images
   end
 end

 # convert the xml to a hash
 Azure::StorageManagement::Serialization.class_eval do
   def self.storage_account_keys_from_xml(storage_xml)
     storage_xml.css('StorageService')
     storage_service_xml = storage_xml.css('StorageService').first
     service_key_xml = storage_service_xml.css('StorageServiceKeys').first
     { url: xml_content(storage_service_xml, 'Url'),
       primary_key: xml_content(service_key_xml, 'Primary'),
       secondary_key: xml_content(service_key_xml, 'Secondary') }
   end
 end

 # support the get storage keys rest api
 module Azure
   module StorageManagement
     class StorageManagementService < BaseManagementService
       def get_storage_account_keys(name)
         request_path = "/services/storageservices/#{name}/keys"
         request = ManagementHttpRequest.new(:get, request_path, nil)
         response = request.call
         Serialization.storage_account_keys_from_xml(response)
       end
     end
   end
 end
{% endhighlight %}

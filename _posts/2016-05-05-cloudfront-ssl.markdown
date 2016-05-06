---
layout: post
title:  "Upload x509 certificate for use in CloudFront"
date:   2016-05-05
thumbnail: gtg_the-crew.png 
image: gtg_sun-over-shoulder.png 
categories: x509 cloudfront cdn aws 
tags: top_level_post
---

CloudFront allows you to upload x509 certs to IAM for use by the CloudFront
CDNs.  Recently mine expired and I needed to upload a new one.
{% highlight bash %}
# Listing the certs shows my existing expired cert. 
$ aws iam  list-server-certificates
{
    "ServerCertificateMetadataList": [
      "ServerCertificateMetadata": {   
        "ServerCertificateId": "AAAAAAAAAAAAAAAAAAAAA",
        "ServerCertificateName": "xplode.io",
        "Expiration": "2014-05-25T04:02:56Z",
        "Path": "/cloudfront/",
        "Arn": "arn:aws:iam::111111111111:server-certificate/cloudfront/xplode.io",
        "UploadDate": "2015-01-24T19:22:30Z"
      }
    ]
}

# The command signature to upload a new cert is:
aws iam upload-server-certificate --server-certificate-name <name to assign to cert> --certificate-body file://cert.pem --private-key file://key.pem --certificate-chain file://chain.pem

# Executing the command returns the new cert json. 
$ aws iam upload-server-certificate --server-certificate-name star.xplode.io --certificate-body file://star.xplode.io.crt --private-key file://star.xplode.io.key --certificate-chain file://star.xplode.io.ca-bundle

{
      "ServerCertificateMetadata": {   
        "ServerCertificateId": "BBBBBBBBBBBBBBBBBBBBB", 
        "ServerCertificateName": "star.xplode.io",
        "Expiration": "2019-05-27T20:19:59Z",
        "Path": "/", 
        "Arn": "arn:aws:iam::111111111111:server-certificate/star.xplode.io",
        "UploadDate": "2016-05-05T18:52:59.054Z"  
      }
}

# Now if I list the certs, both the old and new cert returned.
$ aws iam  list-server-certificates{
    "ServerCertificateMetadataList": [
      "ServerCertificateMetadata": {   
        "ServerCertificateId": "AAAAAAAAAAAAAAAAAAAAA",
        "ServerCertificateName": "xplode.io",
        "Expiration": "2014-05-25T04:02:56Z",
        "Path": "/cloudfront/",
        "Arn": "arn:aws:iam::111111111111:server-certificate/cloudfront/xplode.io",
        "UploadDate": "2015-01-24T19:22:30Z"
      },
      "ServerCertificateMetadata": {   
        "ServerCertificateId": "BBBBBBBBBBBBBBBBBBBBB", 
        "ServerCertificateName": "star.xplode.io",
        "Expiration": "2019-05-27T20:19:59Z",
        "Path": "/", 
        "Arn": "arn:aws:iam::111111111111:server-certificate/star.xplode.io",
        "UploadDate": "2016-05-05T18:52:59.054Z"  
      }
    ]
}

# To use the new certificate when creating a CDN in CloudFront via the AWS API, set aws_certificate_id to the new id: BBBBBBBBBBBBBBBBBBBBB


{% endhighlight %}
<hr>
On a side note, if you have multiple AWS accounts, the --profile option for the
aws command line tool is really cool.

{% highlight bash %}
aws --profile development iam  list-server-certificates
{% endhighlight %}

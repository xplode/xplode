---
layout: post
title:  "Using vim as an IDE on Mac"
date:   
tags: 
---

# What to install, applications, vim plugins and supporting software.

Make sure you have the latest version of vim installed via homebrew, I'm using version 8
brew install vim, although I use macvim.


cd ~/Library/Fonts && curl -fLo "Droid Sans Mono for Powerline Nerd Font Complete.otf" https://raw.githubusercontent.com/ryanoasis/nerd-fonts/master/patched-fonts/DroidSansMono/complete/Droid%20Sans%20Mono%20for%20Powerline%20Nerd%20Font%20Complete.otf
After doing this you will need to setup iterm to use this font.

git clone https://github.com/scrooloose/syntastic.git ~/.vim/bundle/syntastic
git clone https://github.com/vim-airline/vim-airline ~/.vim/bundle/vim-airline
git clone git@github.com:tiagofumo/vim-nerdtree-syntax-highlight.git ~/.vim/bundle/vim-nerdtree-syntax-highlight
git clone https://github.com/ryanoasis/vim-devicons ~/.vim/bundle/vim-devicons
git clone https://github.com/scrooloose/nerdtree.git ~/.vim/bundle/nerdtree


npm install -g csslint
npm install jsonlint -g
npm install htmltidy -g
npm install -g tslint typescript
npm install -g jshint

# Install youcompleteme for code completion.  This requires the installation of
# macvim, clang, java, eclipse, eclim.
git clone https://github.com/VundleVim/Vundle.vim.git ~/.vim/bundle/Vundle.vim
brew update
brew install macvim
brew link macvim
vim --version
mvim
brew install cmake
cd .vim/bundle/YouCompleteMe/
./install.py --clang-completer
git submodule update --init --recursive
./install.py --clang-completer --tern-completer
brew install go --cross-compile-common --gocode-completer --tern-completer
npm install -g typescript
brew cask install java
javac --version
tar -zxvf eclipse-inst-mac64.tar.gz 
cd Eclipse\ Installer.app/
mkdir ../engineering/eclipse
java -jar eclim_2.5.0.jar 
# you'll need the eclipse home
export ECLIPSE_HOME = /Applications/Eclipse.app/Contents/Eclipse/
# start eclim
$ECLIPSE_HOME/eclimd


# install and use commandt
https://github.com/wincent



#### Vim commands related to the plugins and using vim as an ide in general. 


Navigating splits
left  -> <ctrl>h
right -> <ctrl>l
down  -> <ctrl>j
up    -> <ctrl>k

Execute a bash shell command from within vim
!<command>


Open nerdtree from within vim
<ctrl>n<esc>

Open nerdtree file in a vertical split
s

Print the current working directory
:pwd

Change the current working directory to the directory of the currently open file
:cd %:p:h

Reload .vimrc file
:so %

Save the current file under a different name (old file is not deleted)
:saveas <filename>

# CommandT
open the file in a vertical split
<loader>t
<c-v>

# Eunich vim
:Remove: Delete a buffer and the file on disk simultaneously.
:Unlink: Like :Remove, but keeps the now empty buffer.
:Move: Rename a buffer and the file on disk simultaneously.
:Rename: Like :Move, but relative to the current file's containing directory.
:Chmod: Change the permissions of the current file.
:Mkdir: Create a directory, defaulting to the parent of the current file.
:Find: Run find and load the results into the quickfix list.
:Locate: Run locate and load the results into the quickfix list.
:Wall: Write every open window. Handy for kicking off tools like guard.
:SudoWrite: Write a privileged file with sudo.
:SudoEdit: Edit a privileged file with sudo.



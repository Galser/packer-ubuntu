# packer-ubuntu
Vagrant VirtualBox base box with Ubuntu Bionic Beaver 64bit

# Purpose 

This repository attempts to have minimal amount of code that is required to create an Ubuntu Bionic Beaver 64it box using Packer for running in VirtualBox with management by Vagrant

# Prerequisites

1. To download the content of this repository you will need **git command-line tools**(recommended) or **Git UI Client**. To install official command-line Git tools please [find here instructions](https://git-scm.com/book/en/v2/Getting-Started-Installing-Git) for various operation systems. 
2. This box for virtualizaion using **VirtualBox**, download the binaries for your [platform here](https://www.virtualbox.org/wiki/Downloads) and then follow [instructions for installation](https://www.virtualbox.org/manual/ch02.html)
3. For managing of VM (virtual machines) we are going to use **Vagrant**. To install **Vagrant** , please follow instructions here : [official Vargant installation manual](https://www.vagrantup.com/docs/installation/)
4. For creating bas box image we need **Packer** - an open source tool for creating identical machine images for multiple platforms from a single source configuration.  You cam [download for your platform here](https://www.packer.io/downloads.html)  and then [follow this installation instrucion](https://www.packer.io/intro/getting-started/install.html#precompiled-binaries). 

# How to use

# TODO

- [ ] create basic template
- [ ] build first box
- [ ] create initial Vagrant template to run the box
- [ ] tune packer template
- [ ] test box
- [ ] update readme with instructions

# DONE

- [x] create inital readme
- [X] get require ISO links and checksums :
    - [Ubuntu 18.04.3](http://releases.ubuntu.com/18.04.3/ubuntu-18.04.3-live-server-amd64.iso?_ga=2.8014258.1409596254.1568807879-1626201701.1568377158)
    - CRC  : "b9beac143e36226aa8a0b03fc1cbb5921cff80123866e718aaeba4edb81cfa63"
# packer-ubuntu
Vagrant VirtualBox base box with Ubuntu Bionic Beaver 64bit created vai Packer.
For corresponding technologies see [section Prerequisites](#prerequisites)

# Purpose 

This repository attempts to have minimal amount of code that is required to create an Ubuntu Bionic Beaver 64it box using Packer for running in VirtualBox with management by Vagrant

# Prerequisites

1. To download the content of this repository you will need **git command-line tools**(recommended) or **Git UI Client**. To install official command-line Git tools please [find here instructions](https://git-scm.com/book/en/v2/Getting-Started-Installing-Git) for various operation systems. 
2. This box for virtualizaion using **VirtualBox**, download the binaries for your [platform here](https://www.virtualbox.org/wiki/Downloads) and then follow [instructions for installation](https://www.virtualbox.org/manual/ch02.html)
3. For managing of VM (virtual machines) we are going to use **Vagrant**. To install **Vagrant** , please follow instructions here : [official Vargant installation manual](https://www.vagrantup.com/docs/installation/)
4. For creating bas box image we need **Packer** - an open source tool for creating identical machine images for multiple platforms from a single source configuration.  You cam [download for your platform here](https://www.packer.io/downloads.html)  and then [follow this installation instrucion](https://www.packer.io/intro/getting-started/install.html#precompiled-binaries).  In our case Packer is going to take care of installing OS into VM, communicating with it, doing basic provision and preparing for us packed Vagrant box, ready to use.
# How to use

1. To download the copy of the code (*clone* in Git terminology) - go to the location of your choice (normally some place in home folder) and run in terminal ``https://github.com/Galser/packer-ubuntu.git`` ; in case you are using alternative Git Client - please follow appropriate instruction for it and download(*clone*) [this repo](https://github.com/Galser/packer-ubuntu.git). 
2. Previous step should create the folder that contains the copy of repository. Default name is going to be the same as the name of repository e.g. ``packer-ubuntu``. Locate and open it. To do this in UNIX Terminal execute ``cd packer-ubuntu`` 
3. Now to create the Vagrant base box we going to use Packer. Run from command line ``packer -force template.json``
*Note: It is going to take some time, as Packer need to download the ISO image of the operating system, run it, make installation and all required adjustment and then packe everything into format suitable for consuming by Vagrant running with VirtualBox*
In case of successful process completion you should see these several lines : 
> ==> ubuntu-1804-vbox (vagrant): Creating Vagrant box for 'virtualbox' provider
>     ubuntu-1804-vbox (vagrant): Copying from artifact: output-ubuntu-1804-vbox/ubuntu-1804-vbox-disk001.vmdk
>     ubuntu-1804-vbox (vagrant): Copying from artifact: output-ubuntu-1804-vbox/ubuntu-1804-vbox.ovf
>     ubuntu-1804-vbox (vagrant): Renaming the OVF to box.ovf...
>     ubuntu-1804-vbox (vagrant): Compressing: Vagrantfile
>     ubuntu-1804-vbox (vagrant): Compressing: box.ovf
>     ubuntu-1804-vbox (vagrant): Compressing: metadata.json
>     ubuntu-1804-vbox (vagrant): Compressing: ubuntu-1804-vbox-disk001.vmdk
> Build 'ubuntu-1804-vbox' finished.
Now you should have file `ubuntu-1804-vbox.box` in the current folder. This is you Vagrant base box with Ubuntu Bionice Beaver 64Bit.
You can stop here, or do several complimentary step to test run the box. 
5. We going to initialize Vagrant with our fresh box, bring it up , chekc and destroy to free resources.
  1. We need to let know Vagrant about our new box - e.g. add box. To do soe execute : ``vagrant box add ubuntu-1804-vbox ubuntu-1804-vbox.box``
  Output should look like : 
  > ==> box: Box file was not detected as metadata. Adding it directly...
  > ==> box: Adding box 'ubuntu-1804-vbox' (v0) for provider: 
  >  box: Unpacking necessary files from: file:///Users/andrii/labs/skills/packer-ubuntu/ubuntu-1804-vbox.box
  > ==> box: Successfully added box 'ubuntu-1804-vbox' (v0) for 'virtualbox'!
  2. Now we can initialize our Vagrant environment with command : ``vagrant init ubuntu-1804-vbox``. This will create **Vagrantfile** in the current foilder with all the required settings to run this base box. 
  3. To create and provision virtual machine with Vagrant - execute from command line ``vagrant up`` ( *This will utilize settings from preivos step - from Vagrantfile* )  :
  4. At this point VM already up and running , so you can use SSH client to connect to it. For example for Linux and MacOS - execute from command-line : ``vagrant ssh``
    Yous should see Basic Ubuntu greeting at first line , something like this : 
    > Welcome to Ubuntu 18.04.3 LTS (GNU/Linux 4.15.0-55-generic x86_64) 
  5. When you've done with the tests and don't need VM shell anymore - you should exit the SSH session - by executing in command line ``exit``
  6. To completely destroy the VM and free up all your system resource (CPU, memory) you will need to give Vagrant instruction to do so - execute from command line ``vagrant destroy``. Next you should see the question on a new line :
  ``` default: Are you sure you want to destroy the 'default' VM? [y/N]  ```
  Answer 'y' from keyboard, and you are good to go

# TODO

- [ ] update readme with instructions

# DONE

- [x] create inital readme
- [X] get require ISO links and checksums :
    - [Ubuntu 18.04.3](http://releases.ubuntu.com/18.04.3/ubuntu-18.04.3-live-server-amd64.iso?_ga=2.8014258.1409596254.1568807879-1626201701.1568377158)
    - CRC  : "b9beac143e36226aa8a0b03fc1cbb5921cff80123866e718aaeba4edb81cfa63"
- [X] create basic template
- [X] build first box
- [X] create initial Vagrant template to run the box
- [x] tune packer template
- [x] test box

# HINTS

- Starting from Ubuntu Bionic Beaver LTS **RELEASE** ( warning - RELEASE .e.g ) the server ISO to use must be alternative iso, not main one, casper.
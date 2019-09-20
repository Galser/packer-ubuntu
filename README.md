# packer-ubuntu
*Vagrant* **base box**¹ of Ubuntu Bionic Beaver 64bit created with *Packer* for run in *VirtualBox*.
For the corresponding technologies mentioned here see in details [Prerequisite section](#prerequisites)

*¹* - There are a special category of boxes known as "base boxes." These boxes contain the bare minimum required for Vagrant to function, are generally not made by repackaging an existing Vagrant environment (hence the "base" in the "base box").

# Purpose 

This repository attempts to have minimal amount of code that is required to create an Ubuntu Bionic Beaver 64it box using Packer for running in VirtualBox with management by Vagrant.

# Prerequisites

1. To download the content of this repository you will need **git command-line tools**(recommended) or **Git UI Client**. To install official command-line Git tools please [find here instructions](https://git-scm.com/book/en/v2/Getting-Started-Installing-Git) for various operation systems. 
2. This box for virtualizaion uses **VirtualBox**, download the binaries for your [platform here](https://www.virtualbox.org/wiki/Downloads) and then follow [instructions for installation](https://www.virtualbox.org/manual/ch02.html)
3. For managing of VM (virtual machines) we are going to use **Vagrant**. To install **Vagrant** , please follow instructions here : [official Vargant installation manual](https://www.vagrantup.com/docs/installation/)
4. For creating base box image from scratch we need **Packer** - an open source tool for creating identical machine images for multiple platforms from a single source configuration.  You can [download binaries for your platform here](https://www.packer.io/downloads.html)  and then [follow this installation instruction](https://www.packer.io/intro/getting-started/install.html#precompiled-binaries).  In our case Packer is going to take care of installing OS into VM, communicating with it, doing basic provision and preparing for us packed Vagrant box, ready to use.

# How to use

1. To download the copy of the code (*clone* in Git terminology) - go to the location of your choice (normally some place in home folder) and run in terminal `git clone https://github.com/Galser/packer-ubuntu.git` ; in case you are using alternative Git Client - please follow appropriate instruction for it and download(*clone*) [this repo](https://github.com/Galser/packer-ubuntu.git). 
2. Previous step should create the folder that contains copy of repository. Default name is going to be the same as the name of repository e.g. `packer-ubuntu`. Locate and open it. To do this in UNIX Terminal execute `cd packer-ubuntu` 
3. Now to create the Vagrant base box we are going to use Packer and [template](templates.json) with [provision scripts](scripts/provision.sh) provided in this repo. Run from command line `packer -force template.json`
*Note: It is going to take some time, as Packer need to download the full ISO image of the operating system, run it, make installation and all required adjustments and then pack everything into format suitable for consuming by Vagrant running with VirtualBox*
In case of successful process completion you would see these lines :
```
 ==> ubuntu-1804-vbox (vagrant): Creating Vagrant box for 'virtualbox' provider
     ubuntu-1804-vbox (vagrant): Copying from artifact: output-ubuntu-1804-vbox/ubuntu-1804-vbox-disk001.vmdk
     ubuntu-1804-vbox (vagrant): Copying from artifact: output-ubuntu-1804-vbox/ubuntu-1804-vbox.ovf
     ubuntu-1804-vbox (vagrant): Renaming the OVF to box.ovf...
     ubuntu-1804-vbox (vagrant): Compressing: Vagrantfile
     ubuntu-1804-vbox (vagrant): Compressing: box.ovf
     ubuntu-1804-vbox (vagrant): Compressing: metadata.json
     ubuntu-1804-vbox (vagrant): Compressing: ubuntu-1804-vbox-disk001.vmdk     
 Build 'ubuntu-1804-vbox' finished.
```
Now you should have file `ubuntu-1804-vbox.box` in the current folder. This is you Vagrant base box with Ubuntu Bionice Beaver 64Bit. Cogratulations! 
You can stop here, or do several *complimentary steps* to test run the box. 
5. We going to initialize Vagrant with our fresh box, bring it up , check and later destroy to free resources.
  1. We need to let know Vagrant about our new box - e.g. add it. To do so execute : `vagrant box add ubuntu-1804-vbox ubuntu-1804-vbox.box`
  Output should look like : 
  ```
   ==> box: Box file was not detected as metadata. Adding it directly...
   ==> box: Adding box 'ubuntu-1804-vbox' (v0) for provider: 
       box: Unpacking necessary files from: file:///.../packer-ubuntu/ubuntu-1804-vbox.box
   ==> box: Successfully added box 'ubuntu-1804-vbox' (v0) for 'virtualbox'!
   ```
  2. Now we can initialize our Vagrant environment with command : `vagrant init ubuntu-1804-vbox`. This will create **Vagrantfile** in the current foilder with all the required settings to run this base box. 
  3. To create and provision virtual machine with Vagrant - execute from command line `vagrant up` ( *This will utilize settings from previous step - from Vagrantfile* )  :
  4. At this point VM already up and running , so you can use SSH client to connect to it. For example for Linux and MacOS - execute from command-line : `vagrant ssh`
   You should see Basic Ubuntu greeting at first line , something like this : 
  
     ```Welcome to Ubuntu 18.04.3 LTS (GNU/Linux 4.15.0-55-generic x86_64)```
   
   Play around, but remember - this is **base box** - e.g. bare minumum Ubuntu Linux.
  
  5. When you've done with the tests and don't need VM anymore - you should exit the SSH session - by executing in command line `exit`
  6. Now - to completely destroy the VM and free up all your system resource (CPU, memory)  - execute from command line `vagrant destroy`. Next you should see the question on a new line :
  
     ``` default: Are you sure you want to destroy the 'default' VM? [y/N]  ```
  
   Answer `y` from keyboard, and you are good to go

# TODO


# DONE

- [x] create inital readme
- [X] get require ISO links and checksums :
    - [Ubuntu 18.04.3](http://cdimage.ubuntu.com/releases/18.04.3/release/ubuntu-18.04.3-server-amd64.iso)
    - CRC  : "7d8e0055d663bffa27c1718685085626cb59346e7626ba3d3f476322271f573e"
- [X] create basic template
- [X] build first box
- [X] create initial Vagrant template to run the box
- [x] tune packer template
- [x] test box
- [X] update readme with instructions

# HINTS

- Starting from Ubuntu Bionic Beaver LTS **RELEASE** ( warning - RELEASE .e.g ) the server ISO to use must be alternative iso, not main one, casper.

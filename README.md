# packer-ubuntu
*Vagrant* *VirtualBox* **base box**¹ of Ubuntu Bionic Beaver 64bit created with *Packer* .
For the corresponding technologies mentioned here see in details [Prerequisite section](#prerequisites)

*¹* - *There are a special category of boxes known as "base boxes." These boxes contain the bare minimum required for Vagrant to function, are generally not made by repackaging an existing Vagrant environment (hence the "base" in the "base box").*

# Purpose 

This repository attempts to have minimal amount of code that is required to create an Ubuntu Bionic Beaver 64it box using Packer for running in VirtualBox with management by Vagrant.

# Prerequisites

1. To download the content of this repository you will need **git command-line tools**(recommended) or **Git UI Client**. To install official command-line Git tools please [find here instructions](https://git-scm.com/book/en/v2/Getting-Started-Installing-Git) for various operation systems. 
2. This box for virtualizaion uses **VirtualBox**, download the binaries for your [platform here](https://www.virtualbox.org/wiki/Downloads) and then follow [instructions for installation](https://www.virtualbox.org/manual/ch02.html)
3. For managing of VM (virtual machines) we are going to use **Vagrant**. To install **Vagrant** , please follow instructions here : [official Vargant installation manual](https://www.vagrantup.com/docs/installation/)
4. For creating base box image from scratch we need **Packer** - an open source tool for creating identical machine images for multiple platforms from a single source configuration.  You can [download binaries for your platform here](https://www.packer.io/downloads.html)  and then [follow this installation instruction](https://www.packer.io/intro/getting-started/install.html#precompiled-binaries).  In our case Packer is going to take care of installing OS into VM, communicating with it, doing basic provision and preparing for us packed Vagrant box, ready to use.

# How to use

- To download the copy of the code (*clone* in Git terminology) - go to the location of your choice (normally some place in home folder) and run in terminal; in case you are using alternative Git Client - please follow appropriate instruction for it and download(*clone*) [this repo](https://github.com/Galser/packer-ubuntu.git). 
```
git clone https://github.com/Galser/packer-ubuntu.git
```

- Previous step should create the folder that contains copy of repository. Default name is going to be the same as the name of repository e.g. `packer-ubuntu`. Locate and open it.
```
cd packer-ubuntu
```

- Now to create the Vagrant base box we are going to use Packer and [template](templates.json) with [provision scripts](scripts/provision.sh) provided in this repo.
*Note: It is going to take some time, as Packer need to download the full ISO image of the operating system, run it, make installation and all required adjustments and then pack everything into format suitable for consuming by Vagrant running with VirtualBox*
```
packer build template.json
```

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

Now you should have file `ubuntu-1804-vbox.box` in the current folder. This is you Vagrant base box with Ubuntu Bionice Beaver 64Bit. Congratulations! 
You can stop here, or do several *complimentary steps* to test run the box. 

We going to initialize Vagrant with our fresh box, bring it up , check and later destroy to free resources.

- We need to let know Vagrant about our new box - e.g. add it. 
```
vagrant box add --name ubuntu-1804-vbox ubuntu-1804-vbox ubuntu-1804-vbox.box
```

Output should look like : 
```
 ==> box: Box file was not detected as metadata. Adding it directly...
 ==> box: Adding box 'ubuntu-1804-vbox' (v0) for provider: 
     box: Unpacking necessary files from: file:///.../packer-ubuntu/ubuntu-1804-vbox.box
 ==> box: Successfully added box 'ubuntu-1804-vbox' (v0) for 'virtualbox'!
 ```
 
- Now we can initialize our Vagrant environment. This will create **Vagrantfile** in the current folder with all the required settings to run this base box. 
 ```
 vagrant init ubuntu-1804-vbox
 ```
 
- To create and provision virtual machine with Vagrant - execute from command line :
> This will utilize settings from previous step - from Vagrantfile
 ```
 vagrant up
 ```


- At this point VM already up and running , so you can use SSH client to connect to it. For example for Linux and MacOS - execute from command-line : 
 ```
 vagrant ssh
 ```

You should see Basic Ubuntu greeting at first line , something like this : 
 ```
 Welcome to Ubuntu 18.04.3 LTS (GNU/Linux 4.15.0-55-generic x86_64)
 ```

Play around, but remember - this is **base box** - e.g. bare minimum Ubuntu Linux.

- When you've done with the tests and don't need VM anymore - you should exit the SSH session - by executing in command line :
 ```
 exit
 ```

- Now - to completely destroy the VM and free up all your system resource (CPU, memory)  - execute from command line :
 ```
 vagrant destroy
 ``` 
 
 Next you should see the question on a new line :
 ```
 default: Are you sure you want to destroy the 'default' VM? [y/N]
 ```
 
 Answer `y` from keyboard, and you are good to go
 
 
## Add-On : Publishing box on Vagrant Cloud

[Vagrant Cloud](https://app.vagrantup.com/) is a service that provides the following features for Vagrant:
[Vagrant Box Catalog](https://www.vagrantup.com/docs/vagrant-cloud/boxes/catalog.html), [Vagrant Box Creation](https://www.vagrantup.com/docs/vagrant-cloud/boxes/create.html) and [Vagrant Box Versioning](https://www.vagrantup.com/docs/vagrant-cloud/boxes/lifecycle.html). So it serves a public, searchable index of Vagrant boxes. It's easy to find boxes you can use with Vagrant that contain the technologies you need for a Vagrant environment. You can also upload (publish) your own box, and in addition  - have versioning - so that members of your team using Vagrant can update the underlying box easily, and the people who create boxes can push fixes and communicate these fixes efficiently. 
- To publish box in Vagrant cloud you will need a corrsponding account. If you don't have one, please [register here](https://app.vagrantup.com/account/new
)
- Login by executing the command in the folder with clone of this repository : 
  ```
  vagrant cloud auth login
  ```
  You will need to fill username and password. And just accept for now default token name.
  ```
  Vagrant Cloud username or email: [your_user_name] 
  Password (will be hidden): 
  Token description (Defaults to "Vagrant login from ***.home"):
  ```
- To publish box (you will need organization - if you just have created account - it will be the same as your user name), enter :
  ```
  vagrant cloud publish YOUR_OGRANIZATION/ubuntu-1804-vbox 0.0.1 virtualbox ubuntu-1804-vbox.box
  ```
  You are going to see some output like this : 
  ```
  You are about to publish a box on Vagrant Cloud with the following options:
  YOUR_OGRANIZATION/ubuntu-1804-vbox:   (v0.0.1) for provider 'virtualbox'
  Do you wish to continue? [y/N] y
  ```
  Answer `y`, and wait. Vagrant going to upload the box, so, depending from your Internt connection it wil ltake some time.
  At the end you should see a message starting with : 
  ```
  Complete! Published YOUR_OGRANIZATION/ubuntu-1804-vbox
  tag:        YOUR_OGRANIZATION/ubuntu-1804-vbox
  username:   YOUR_USER_NAME
  name:       ubuntu-1804-vbox
  ```
- Now you can go and check online, in [dashboard of Vagrant cloud](https://app.vagrantup.com/) for the box with name *ubuntu-1804-vbox*.
  Please pay attention, that at first your box is going to have no released version, you should see a banner with yellow background warning on the page :
  ```
  This box has no released versions. It will not be available from vagrant box add, nor will it show up in search results.
  ```
  So, if you want to be able to add this box on another computer, and for this image to be searchable, you need to make some additional steps : 
  - Click on the name of the box in Dashboard
  - In the new screen, ( that will have URL like this : '''https://app.vagrantup.com/YOUR_ORGANITZATION/boxes/ubuntu-1804-vbox''') find the version you've just deployed - 0.0.1, next to it going to be remark *unreleased* and a blue button **Release**, press it.
  - Next screen will appear - where you can fill the details for you box - for example enter - description in [Markdown syntax](https://guides.github.com/features/mastering-markdown/), when you done - press button "Update version"
  - Now to actually *release* box - you need to press button **"Release version"** in the top-left corner of the main screen
  - And you are going to be back to the box information scren, where at the top you can see new banner on green background : 
  ```
  Successfully released
  ```
  - That's it. You've just published box in Vagrant cloud, and it is avaible for search and usage.
  
  
 

# TODO


# DONE
- [x] create inital readme
- [X] get require ISO links and checksums
- [X] create basic template
- [X] build first box
- [X] create initial Vagrant template to run the box
- [x] tune packer template
- [x] test box
- [X] update readme with instructions
- [X] add steps to publish box on vagrant cloud using `vagrant cloud publish`

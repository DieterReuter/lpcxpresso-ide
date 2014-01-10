lpcxpresso-ide
==============

LPCXpresso IDE based upon Ubuntu 12.04 (Precise 64Bit)
Using: LPCXpresso_6.1.2

# lpcxpresso-ide

Easy start developing with LPCXpresso IDE running on Ubuntu 12.04 VM.

# Installation
You will need [Vagrant](http://vagrantup.com) and [VirtualBox](http://virtualbox.org) to install the Ubuntu Box. It will then install the LPCXpresse IDE inside and some other tools you may need to start developing.

## Preparing your Windows Host
If you do not already have Vagrant and VirtualBox installed, you may use following script that will do this for you. I prefer [Chocolatey](http://chocolatey.org) to install all my software on Windows machines. Open a command shell or type **WindowsKey+R** and copy and paste following command

    @powershell -NoProfile -ExecutionPolicy unrestricted -Command "((new-object net.webclient).DownloadFile('https://raw.github.com/DieterReuter/lpcxpresso-ide/install/windows.bat', '%Temp%\windows.bat'))" && %Temp%\windows.bat

Enter the admin user and password when prompted in UAC dialogs. It will install the DotNet runtime 4 which is needed by Chocolatey, then VirtualBox and Vagrant and then the Vagrantfile from this repo.

## Preparing the LPCXpresso IDE Ubuntu box
The LPCXpresso IDE will be installed into an Ubuntu 12.04 box. Just clone this repo and call vagrant up with the following commands:

    git clone https://github.com/DieterReuter/lpcxpresso-ide.git
    cd lpcxpresso-ide
    vagrant up

# Licensing
Copyright (c) 2014 Dieter Reuter

MIT License, see LICENSE.txt for more details.

# -*- mode: ruby -*-
# vi: set ft=ruby :

#---script---
$script = <<SCRIPT

# switch to German keyboard layout
sudo -E sed -i 's/"us"/"de"/g' /etc/default/keyboard
export DEBIAN_FRONTEND=noninteractive
sudo -E apt-get install -y console-common
sudo -E install-keymap de
     
sudo -E apt-get update -y
sudo -E apt-get upgrade -y
sudo -E apt-get install -y --no-install-recommends ubuntu-desktop
sudo -E apt-get install -y gnome-panel
sudo -E apt-get install -y unity-lens-applications

#### install freenx-server
###sudo -E apt-get install -y python-software-properties                                                                                 
###sudo -E apt-get install -y unity-lens-applications
###sudo -E apt-add-repository -y ppa:freenx-team
###sudo -E apt-get update -y
###sudo -E apt-get install -y freenx-server
####sudo -E /usr/lib/nx/nxsetup --install

# install Chromium  browser
sudo -E apt-get install -y chromium-browser

# setup for LPCXpresso IDE
sudo -E apt-get install -y linux32 ia32-libs
sudo -E -u vagrant mkdir -p /vagrant/lpcxpresso
cd /vagrant/lpcxpresso
if [ ! -f Installer_LPCXpresso_6.1.2_177_Linux-x86.tar.gz ]; then
  echo "wget https://s3.amazonaws.com/LPCXpresso6/Installer_LPCXpresso_6.1.2_177_Linux-x86.tar.gz ..."
  sudo -E -u vagrant wget -q https://s3.amazonaws.com/LPCXpresso6/Installer_LPCXpresso_6.1.2_177_Linux-x86.tar.gz
fi
echo "Extract Installer_LPCXpresso_6.1.2_177_Linux-x86.tar.gz to Desktop..."
sudo -E -u vagrant mkdir -p /home/vagrant/Desktop
sudo -E -u vagrant tar xvfz Installer_LPCXpresso_6.1.2_177_Linux-x86.tar.gz -C /home/vagrant/Desktop
echo "AutoInstall Installer_LPCXpresso_6.1.2_177_Linux-x86 ..."
sudo -E /home/vagrant/Desktop/Installer_LPCXpresso_6.1.2_177_Linux-x86 --mode silent

# setup development: GIT, VIM, ...
sudo -E apt-get install -y vim git
sudo -E -u vagrant gconftool -s /apps/gnome-terminal/profiles/Default/use_system_font -t bool false

# start X server
sudo -E service lightdm restart

SCRIPT
#---script---


# Vagrantfile API/syntax version. Don't touch unless you know what you're doing!
VAGRANTFILE_API_VERSION = "2"

Vagrant.configure(VAGRANTFILE_API_VERSION) do |config|
  config.vm.hostname = "lpcxpresso-ide-precise64"
  config.vm.box = "precise64"
  config.vm.box_url = "http://files.vagrantup.com/precise64.box"
  ###config.vm.box = "precise64-ss"
  ###config.vm.box_url = "J:/GitHub/stefanscherer/basebox-packer/virtualbox/ubuntu1204x64-desktop.box"
  
  config.vm.provider :virtualbox do |vb|
    vb.gui = true
    # Use VBoxManage to customize the VM. For example to change memory:
    vb.customize ["modifyvm", :id, "--memory", "1024", "--cpus", "1"]
    vb.customize ["modifyvm", :id, "--usb", "on", "--clipboard", "bidirectional"]
  end

  # Provision using the shell to install
  config.vm.provision "shell", inline: $script
  
end

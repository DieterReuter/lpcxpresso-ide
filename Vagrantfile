# -*- mode: ruby -*-
# vi: set ft=ruby :

#---script---
#(running as user="root")
$script = <<SCRIPT

# set ubuntu download mirror
sudo sed -i 's,http://us.archive.ubuntu.com/ubuntu/,http://ftp.fau.de/ubuntu/,' /etc/apt/sources.list
sudo sed -i 's,http://security.ubuntu.com/ubuntu,http://ftp.fau.de/ubuntu,' /etc/apt/sources.list
export DEBIAN_FRONTEND=noninteractive
apt-get update -y

# switch to German keyboard layout
sed -i 's/"us"/"de"/g' /etc/default/keyboard
apt-get install -y console-common
install-keymap de

# set timezone to German timezone
echo "Europe/Berlin" | tee /etc/timezone
dpkg-reconfigure -f noninteractive tzdata

# update/upgrade and install Ubuntu desktop
apt-get upgrade -y
apt-get install -y --no-install-recommends ubuntu-desktop
apt-get install -y gnome-panel
apt-get install -y unity-lens-applications
sudo -E -u vagrant gconftool -s /apps/gnome-terminal/profiles/Default/use_system_font -t bool false

# start desktop (using autologin for user "vagrant")
echo "autologin-user=vagrant" | tee -a /etc/lightdm/lightdm.conf
service lightdm restart

#### install freenx-server
###sudo -E apt-get install -y python-software-properties                                                                                 
###sudo -E apt-get install -y unity-lens-applications
###sudo -E apt-add-repository -y ppa:freenx-team
###sudo -E apt-get update -y
###sudo -E apt-get install -y freenx-server
####sudo -E /usr/lib/nx/nxsetup --install

# install Chromium  browser
apt-get install -y chromium-browser

# setup for LPCXpresso IDE
apt-get install -y linux32 ia32-libs
if [ ! -f /usr/share/applications/lpcxpresso-program.desktop ]; then
  sudo -E -u vagrant mkdir -p /vagrant/lpcxpresso
  cd /vagrant/lpcxpresso
  if [ ! -f Installer_LPCXpresso_6.1.2_177_Linux-x86.tar.gz ]; then
    echo "wget https://s3.amazonaws.com/LPCXpresso6/Installer_LPCXpresso_6.1.2_177_Linux-x86.tar.gz ..."
    sudo -E -u vagrant wget -q https://s3.amazonaws.com/LPCXpresso6/Installer_LPCXpresso_6.1.2_177_Linux-x86.tar.gz
  fi
  echo "Extract Installer_LPCXpresso_6.1.2_177_Linux-x86.tar.gz to Desktop..."
  sudo -E -u vagrant mkdir -p /home/vagrant/Desktop
  if [ ! -f /home/vagrant/Desktop/Installer_LPCXpresso_6.1.2_177_Linux-x86 ]; then
    sudo -E -u vagrant tar xvfz Installer_LPCXpresso_6.1.2_177_Linux-x86.tar.gz -C /home/vagrant/Desktop
  fi
  echo "AutoInstall Installer_LPCXpresso_6.1.2_177_Linux-x86 ..."
  /home/vagrant/Desktop/Installer_LPCXpresso_6.1.2_177_Linux-x86 --mode silent
fi

# setup development: GIT, VIM/GVIM, ...
apt-get install -y git vim vim-gnome

# create Launcher with our preferred applications
# (installed Applications see /usr/share/applications/*.desktop)
sudo -E -u vagrant DISPLAY=:0.0 gsettings set com.canonical.Unity.Launcher favorites "['nautilus-home.desktop', 'lpcxpresso-program.desktop', 'chromium-browser.desktop', 'gnome-terminal.desktop', 'gvim.desktop', 'ubuntu-software-center.desktop', 'gnome-control-center.desktop']"

# setup VBox Guest Additions
/etc/init.d/vboxadd-x11 setup
service lightdm restart

SCRIPT
#---script---


# Vagrantfile API/syntax version. Don't touch unless you know what you're doing!
VAGRANTFILE_API_VERSION = "2"

Vagrant.configure(VAGRANTFILE_API_VERSION) do |config|
  config.vm.hostname = "lpcxpresso-ide-precise64"
  config.vm.box = "precise64"
  config.vm.box_url = "http://files.vagrantup.com/precise64.box"
  ###config.vm.box = "precise64-ops"
  ###config.vm.box_url = "http://opscode-vm-bento.s3.amazonaws.com/vagrant/virtualbox/opscode_ubuntu-12.04_chef-provisionerless.box"
  ###config.vm.box = "precise64-ss"
  ###config.vm.box_url = "J:/GitHub/stefanscherer/basebox-packer/virtualbox/ubuntu1204x64-desktop.box"
  
  config.vm.provider :virtualbox do |vb|
    vb.gui = true
    # Use VBoxManage to customize the VM. For example to change memory:
    vb.customize ["modifyvm", :id, "--memory", "1024", "--cpus", "1"]
    vb.customize ["modifyvm", :id, "--clipboard", "bidirectional"]
    vb.customize ["modifyvm", :id, "--usb", "on"]
  end

  # Provision using the shell to install
  config.vm.provision "shell", inline: $script
  
end

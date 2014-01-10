# -*- mode: ruby -*-
# vi: set ft=ruby :

# Vagrantfile API/syntax version. Don't touch unless you know what you're doing!
VAGRANTFILE_API_VERSION = "2"

# the install script to provision the Ubuntu box with minimal desktop and tools
# (script is running as "root")
$script = <<SCRIPT

# switch to German keyboard layout
#sudo setxkbmap de
#sudo -u vagrant setxkbmap de
sudo sed -i 's/"us"/"de"/g' /etc/default/keyboard

sudo apt-get update -y
sudo apt-get install -y --no-install-recommends ubuntu-desktop
sudo apt-get install -y --no-install-recommends gnome-panel
sudo apt-get install -y unity-lens-applications

# get Chromium as default browser
sudo apt-get install -y chromium-browser

# setup for LPCXpresso
sudo apt-get install -y linux32 ia32-libs
sudo -u vagrant mkdir -p /vagrant/lpcxpresso
cd /vagrant/lpcxpresso
if [ ! -f Installer_LPCXpresso_6.1.2_177_Linux-x86.tar.gz ]; then
  echo "wget https://s3.amazonaws.com/LPCXpresso6/Installer_LPCXpresso_6.1.2_177_Linux-x86.tar.gz ..."
  sudo -u vagrant wget -q https://s3.amazonaws.com/LPCXpresso6/Installer_LPCXpresso_6.1.2_177_Linux-x86.tar.gz
fi
echo "Extract Installer_LPCXpresso_6.1.2_177_Linux-x86.tar.gz to Desktop..."
sudo -u vagrant mkdir -p /home/vagrant/Desktop
sudo -u vagrant tar xvfz Installer_LPCXpresso_6.1.2_177_Linux-x86.tar.gz -C /home/vagrant/Desktop
echo "AutoInstall Installer_LPCXpresso_6.1.2_177_Linux-x86 ..."
sudo /home/vagrant/Desktop/Installer_LPCXpresso_6.1.2_177_Linux-x86 --mode silent

# setup development: GIT, VIM, ...
sudo apt-get install -y vim git
sudo -u vagrant gconftool -s /apps/gnome-terminal/profiles/Default/use_system_font -t bool false

# start X server
#sudo service lightdm restart

SCRIPT


Vagrant.configure(VAGRANTFILE_API_VERSION) do |config|
  config.vm.box = "precise64"
  config.vm.box_url = "http://files.vagrantup.com/precise64.box"
  config.vm.hostname = "lpcxpresso-ide-precise64"
  
  config.vm.provision "shell", inline: $script
  config.vm.provider :virtualbox do |vb|
    vb.gui = true
    # Use VBoxManage to customize the VM. For example to change memory:
    vb.customize ["modifyvm", :id, "--memory", "1024", "--cpus", "1"]
    vb.customize ["modifyvm", :id, "--usb", "on", "--clipboard", "bidirectional"]
  end

end

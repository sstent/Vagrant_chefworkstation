# -*- mode: ruby -*-

# vi: set ft=ruby :

VAGRANTFILE_API_VERSION = "2"

$chefscript = <<CHEFSCRIPT
# Set verbose
set -v

# Set exit on error
set -e

apt-get -y install curl

curl -L https://www.opscode.com/chef/install.sh | sudo bash
echo 'export PATH="/opt/chef/embedded/bin:$PATH"' >> ~/.bash_profile && source ~/.bash_profile


echo "All done!"
CHEFSCRIPT

Vagrant.configure(VAGRANTFILE_API_VERSION) do |config|

  config.vm.box = "precise64"

  #shared folders
  config.vm.synced_folder "chef", "/home/vagrant/.chef"
  config.vm.synced_folder "cookbooks", "/home/vagrant/cookbooks"

  # Begin workstation
  config.vm.define "workstation" do |workstation_config|
    workstation_config.vm.hostname = "workstation"

    workstation_config.vm.provision "shell", inline: $chefscript

    workstation_config.vm.provider "vmware_fusion" do |v|
        v.vmx["memsize"] = "512"
        v.vmx["numvcpus"] = "1"
    end

    workstation_config.vm.provider "virtualbox" do |v|
        v.customize ["modifyvm", :id, "--memory", "512"]
        v.customize ["modifyvm", :id, "--cpus", "1"]
    end
  end
  # End workstation

end

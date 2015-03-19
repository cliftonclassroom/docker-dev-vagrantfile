Vagrant.configure(2) do |config|
  config.vm.box = "ubuntu/trusty64"
  config.vm.network "private_network", ip: "192.168.58.103"

  # Share an additional folder to the guest VM. The first argument is
  # the path on the host to the actual folder. The second argument is
  # the path on the guest to mount the folder. And the optional third
  # argument is a set of non-required options.
  config.vm.synced_folder ".", "/magento-deployment"

  config.vm.provider "virtualbox" do |vb|
    vb.memory = "4096"
    vb.cpus = 8
    vb.name = "ubuntu-14.10-docker-vm"
  end

  config.vm.provision "shell", inline: <<-SHELL
    sudo apt update
    sudo apt install curl
    sudo apt-get -y autoremove puppet chef
    curl -sSL https://get.docker.com/ubuntu/ | sudo sh
    sudo sed -i.bak 's!.*DOCKER_OPTS=.*!DOCKER_OPTS="-H tcp://0.0.0.0:2375 -H unix:///var/run/docker.sock"!' /etc/default/docker
    sudo service docker restart
    # to use 14.10
    sudo sed -i.bak 's/Prompt=lts/Prompt=normal/' /etc/update-manager/release-upgrades
    sudo do-release-upgrade -d -f DistUpgradeViewNonInteractive
    sudo shutdown -r now
  SHELL
end

# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure("2") do |config|

  config.vm.box = "generic/ubuntu2004"

  config.disksize.size = '50GB'

  config.vm.box_check_update = false
  config.vm.network "private_network", ip: "192.168.56.33"
  config.ssh.insert_key = false
  
  config.vm.hostname = "terraform.local"
  config.vm.provider "virtualbox" do |vb|
    vb.gui = false
    vb.memory = "4096"
    vb.cpus = 2
    vb.customize ["modifyvm", :id, "--groups", "/terraform"]

    vb.name = "terraform_33"
  end

	config.vm.provision :docker

  config.vm.provision "shell", inline: <<-SHELL
    sed -i 's/^PasswordAuthentication no/PasswordAuthentication yes/g' /etc/ssh/sshd_config
    systemctl restart sshd.service

    usermod -a -G  docker vagrant

    apt update -y && apt upgrade -y

    curl -L https://github.com/docker/compose/releases/latest/download/docker-compose-$(uname -s)-$(uname -m) \
     -o /usr/local/bin/docker-compose \
      && chmod a+x /usr/local/bin/docker-compose

    wget -O /usr/local/bin/ctop https://github.com/bcicen/ctop/releases/download/v0.7.5/ctop-0.7.5-linux-amd64  \
      && chmod +x /usr/local/bin/ctop

  SHELL

end

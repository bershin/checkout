# -*- mode: ruby -*-
# vi: set ft=ruby :

# Every Vagrant development environment requires a box. You can search for
# boxes at https://atlas.hashicorp.com/search.
BOX_IMAGE1 = "centos/7"
BOX_IMAGE2 = "samdoran/rhel7"
NODE_COUNT = 2

Vagrant.configure("2") do |config|
  config.vm.define "control" do |subconfig|
    subconfig.vm.box = BOX_IMAGE2
    subconfig.vm.hostname = "control"
    subconfig.vm.network :private_network, ip: "10.0.0.10"
  end
  
  (1..NODE_COUNT).each do |i|
    config.vm.define "manage#{i}" do |subconfig|
      subconfig.vm.box = BOX_IMAGE1
      subconfig.vm.hostname = "manage#{i}"
      subconfig.vm.network :private_network, ip: "10.0.0.#{i + 10}"
    end
  end

  # Install avahi on all machines  
  config.vm.provision "shell", inline: <<-SHELL
#   yum install -y avahi-daemon libnss-mdns
	sed -i 's/PasswordAuthentication no/PasswordAuthentication yes/g' /etc/ssh/sshd_config
	systemctl restart sshd
	useradd -g vagrant bjohn
	echo 'ca272dcb' | passwd --stdin bjohn
	mkdir /home/bjohn/.ssh
	chown bjohn.vagrant /home/bjohn/.ssh
	cat /vagrant/id_rsa.pub > /home/bjohn/.ssh/authorized_keys
	chown bjohn.vagrant /home/bjohn/.ssh/authorized_keys
	chmod 600 /home/bjohn/.ssh/authorized_keys
	yum install vim git -y
  SHELL
end

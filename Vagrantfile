# -*- mode: ruby -*-
# vi: set ft=ruby :

BOX_IP        = "192.168.33.10"

aliases = {
  "#{BOX_IP}" => [
    'nginx.demo.com',
  ],
}

def prompt(*args)
  print(*args)
  STDIN.gets
end

Vagrant.configure(2) do |config|
  # Required Plugin warnings 
  [
    { :name => "vagrant-virtual-hostsupdater", :version => ">= 1.2.0" },
  ].each do |plugin|
    if not Vagrant.has_plugin?(plugin[:name], plugin[:version])
      decision = prompt "#{plugin[:name]} #{plugin[:version]} is required. Please run `vagrant plugin install #{plugin[:name]}` \nDo you want to halt process? [y/N]. Answer: "
      if decision.chomp() != "N"
        exit!
      end
    end
  end
end

Vagrant.configure("2") do |config|
  config.vm.box = "geerlingguy/centos7"
  config.vm.box_version = "1.2.21"

  config.virtualhostsupdater.aliases = aliases

  # Create a private network, which allows host-only access to the machine
  # using a specific IP.
  config.vm.network "private_network", ip: "192.168.33.10"

  config.vm.provider "virtualbox" do |vb|
    vb.gui = false
  
    vb.cpus = "2"
    vb.memory = "4096"
  end

  config.vm.provision "shell", inline: <<-SHELL
    yum install -y epel-release
    yum install -y nginx
    systemctl enable nginx
    systemctl start nginx
  SHELL
end

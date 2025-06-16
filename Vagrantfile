# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure("2") do |config|
  config.vm.synced_folder '.', '/vagrant', disabled: true
  
  # Setting up Kali Linux VM as a attack machine
  config.vm.define "attacker" do |attacker|
    attacker.vm.box = "kalilinux/rolling"
    attacker.vm.hostname = "attacker"
    attacker.vm.network "private_network", ip: '192.168.5.2'
    attacker.vm.provider "virtualbox" do |v|
      v.name = "attacker"
      #v.gui = true
      v.cpus = 4
      v.memory = 4096
    end
    attacker.vm.provision "shell", inline: <<-SHELL
      # set up IP address (like seriously why is this not done automatically?)
      ip addr add "192.168.5.2/24" dev "eth1"
    SHELL
  end

  # Setting up DVWA VM (Ubuntu 22.04) as a vulnerable web application
  # for demonstrating Drive-by Compromises
  config.vm.define "dvwa" do |dvwa|
    dvwa.vm.box = "bento/ubuntu-22.04"
    dvwa.vm.box_version = "202502.21.0"

    dvwa.vm.hostname = "dvwa"
    dvwa.vm.network "private_network", ip: '192.168.5.4'
    dvwa.vm.network "forwarded_port", guest: 80, host: 8080

    dvwa.vm.provider "virtualbox" do |v|
      v.name = "dvwa"
      v.gui = false
      v.memory = 2048
    end
    
    dvwa.vm.provision "shell", inline: <<-SHELL
      apt-get update
      sudo bash -c "$(curl --fail --show-error --silent --location https://raw.githubusercontent.com/IamCarron/DVWA-Script/main/Install-DVWA.sh)"
    SHELL
  end

  # Setting up a gateway VM (Linux) for routing traffic
  # between the attacker and victim VMs
  config.vm.define "router" do |gateway|
    gateway.vm.box = "bento/ubuntu-24.04"
    gateway.vm.box_version = "202502.21.0"
    gateway.vm.hostname = "router"
    gateway.vm.network "private_network", ip: '192.168.4.254'
    gateway.vm.network "private_network", ip: '192.168.5.254'

    gateway.vm.provider "virtualbox" do |v|
      v.name = "router"
      v.cpus = 2
      v.memory = 2048
    end

    gateway.vm.provision "shell", inline: <<-SHELL
      # Enable IP forwarding
      #echo 1 > /proc/sys/net/ipv4/ip_forward
      # Install Suricata
      apt-get install software-properties-common
      add-apt-repository ppa:oisf/suricata-stable
      apt-get update
      apt-get install suricata
      apt-get install -y suricata
    SHELL
  end

  # Setting up a victim VM (Windows XP) for demonstrating
  # Drive-by Compromises,
  # exploiting vulnerabilities in Internet Explorer
  config.vm.define "victim" do |victim|
    victim.vm.box = "dvgamerr/win-xp-sp3"
    victim.vm.box_version = "2.0"
    victim.vm.hostname = "victim"
    victim.vm.network "private_network", ip: '192.168.4.4'
    victim.ssh.username = 'vagrant'
    victim.ssh.password = 'vagrant'

    victim.vm.provider "virtualbox" do |v|
      v.name = "victim(Windows XP)"
      v.cpus = 2
      v.memory = 2048
    end

    victim.vm.provision "shell", inline: <<-SHELL
      # Putting target text file to be exfiltrated
      echo "Advanced Cybersecurity Top Secret." > "C:/Users/vagrant/secret.txt"
    SHELL
  end

end
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
  end

  # Setting up DVWA VM (Ubuntu 22.04) as a vulnerable web application
  # for demonstrating Drive-by Compromises
  config.vm.define "dvwa" do |dvwa|
    dvwa.vm.box = "bento/ubuntu-22.04"
    dvwa.vm.box_version = "202502.21.0"

    dvwa.vm.hostname = "dvwa"
    dvwa.vm.network "private_network", ip: '192.168.5.5'
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

  # Setting up a victim VM (Windows XP) for demonstrating
  # Drive-by Compromises,
  # exploiting vulnerabilities in Internet Explorer
  config.vm.define "victim" do |victim|
    victim.vm.box = "dvgamerr/win-xp-sp3"
    victim.vm.box_version = "2.0"
    victim.vm.hostname = "victim"
    victim.vm.network "private_network", ip: '192.168.5.3'
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

  # Setting up a victim VM (Linux)
  #config.vm.define "victim-linux" do |victim_linux|
  #  victim_linux.vm.box = "generic/ubuntu2010"
  #  victim_linux.vm.box_version = "202502.21.0"
  #  victim_linux.vm.hostname = "victim-linux"
  #  victim_linux.vm.network "private_network", ip: '192.168.5.4'
#
  #  victim_linux.vm.provider "virtualbox" do |v|
  #    v.name = "victim-linux"
  #    v.cpus = 2
  #    v.memory = 2048
  #  end
  #  victim_linux.vm.provision "shell", inline: <<-SHELL
  #    # Putting target text file to be exfiltrated
  #    echo "Advanced Cybersecurity Top Secret." > "C:/Users/vagrant/secret.txt"
  #    # Setup graphical interface for victim Linux
  #    apt-get update
  #    apt-get install -y ubuntu-desktop
  #    reboot now
  #  SHELL
  #end
  
  # Setting up Metasploitable3 VM
  #config.vm.define "ub1404" do |ub1404|
  #  ub1404.vm.box = "rapid7/metasploitable3-ub1404"
  #  ub1404.vm.hostname = "metasploitable3-ub1404"
  #
  #  ub1404.vm.network "private_network", ip: '192.168.5.4'
  # 
  #  ub1404.vm.provider "virtualbox" do |v|
  #    v.name = "Metasploitable3-ub1404"
  #    v.memory = 2048
  #  end
  #end

  #config.vm.define "win2k8" do |win2k8|
  #  # Base configuration for the VM and provisioner
  #  win2k8.vm.box = "rapid7/metasploitable3-win2k8"
  #  win2k8.vm.hostname = "metasploitable3-win2k8"
  #  win2k8.vm.communicator = "winrm"
  #  win2k8.winrm.retry_limit = 60
  #  win2k8.winrm.retry_delay = 10
#
  #  win2k8.vm.network "private_network", ip: '192.168.5.5'
#
  #  # Configure Firewall to open up vulnerable services
  #  case ENV['MS3_DIFFICULTY']
  #    when 'easy'
  #      win2k8.vm.provision :shell, inline: "C:\\startup\\disable_firewall.bat"
  #    else
  #      win2k8.vm.provision :shell, inline: "C:\\startup\\enable_firewall.bat"
  #      win2k8.vm.provision :shell, inline: "C:\\startup\\configure_firewall.bat"
  #  end
#
  #  # Insecure share from the Linux machine
  #  win2k8.vm.provision :shell, inline: "C:\\startup\\install_share_autorun.bat"
  #  win2k8.vm.provision :shell, inline: "C:\\startup\\setup_linux_share.bat"
  #  win2k8.vm.provision :shell, inline: "rm C:\\startup\\*" # Cleanup startup scripts
  #end

end
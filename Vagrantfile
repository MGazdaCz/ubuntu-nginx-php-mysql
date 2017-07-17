# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure("2") do |config|
  config.vm.box = "ubuntu/xenial64"

  # Disable automatic box update checking. If you disable this, then
  # boxes will only be checked for updates when the user runs
  # `vagrant box outdated`. This is not recommended.
  config.vm.box_check_update = false

  # nastaveni site
  config.vm.network "private_network", ip: "192.168.44.10"

  #config.vm.network "public_network", auto_config: false
  #config.vm.provision "shell",wget
  #  run: "always",
  #  inline: "ifconfig eth1 192.168.202.70 netmask 255.255.255.0 up"
  #config.vm.provision "shell",
  #  run: "always",
  #  inline: "route add default gw 192.168.202.1"

  # Share an additional folder to the guest VM. The first argument is
  # the path on the host to the actual folder. The second argument is
  # the path on the guest to mount the folder. And the optional third
  # argument is a set of non-required options.
  #config.vm.synced_folder "./", "/var/www/html", type: "smb", mount_options: ["nolock,dir_mode=0777,file_mode=0666"], owner: "www-data", group: "www-data"
  #config.vm.synced_folder "./", "/var/www/html", type: "virtualbox"

  # fix chyby "stdin: is not a tty"
  config.ssh.shell = "bash -c 'BASH_ENV=/etc/profile exec bash'"

  # Konfigurace virtualni masiny VirtualBoxu
  config.vm.provider "virtualbox" do |vb|
    # Nazev virtualni masiny
    vb.name = "DevelServer"

    # Display the VirtualBox GUI when booting the machine
    #vb.gui = true

    # pocet CPU (vlaken)
    vb.cpus = 2

    # Velikost operacni pameti
    vb.memory = "1024"
  end

  # Spusteni skriptu pred Ansiblem
  config.vm.provision "shell", inline: <<-SHELL
    # instalace pozadovanych aplikaci pro ansible
    apt-get install zip unzip jq -y
  SHELL

  # Aktualizace nastaveni serveru pomoci Ansible playbooku
  config.vm.provision "ansible_local" do |ansible|
    ansible.galaxy_role_file = "./ansible/requirements.yml"
    ansible.galaxy_roles_path = "./temp/ansible/"
    ansible.playbook = "./ansible/playbook.yml"
  end

end

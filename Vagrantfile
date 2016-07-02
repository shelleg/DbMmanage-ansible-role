# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure(2) do |config|
  #config.vm.box = "centos/7"
  #config.vm.box = "bento/ubuntu-16.04"
  #config.vm.box_check_update = false

  # network settings
  # config.vm.network "forwarded_port", guest: 8000, host: 80
  #config.vm.network "private_network", ip: "172.16.1.2"

  # provision
  #config.vm.provision :shell, :path => "python_env.sh"
  #config.vm.provision :ansible do |ansible|
   #
    #    ansible.playbook = "playbook.yml"
    #    ansible.sudo = "true"
    #    ansible.sudo_user = "root"
    #    ansible.host_key_checking = "false"
    #    ansible.verbose = "-vvv"
    #  end

  config.vm.define "centos" do |centos|
    centos.vm.network "private_network", type: "dhcp"
    centos.vm.box = "centos/7"
    centos.vm.provision :ansible do |ansible|
        ansible.playbook = "playbook.yml"
        ansible.sudo = "true"
        ansible.sudo_user = "root"
        ansible.host_key_checking = "false"
        ansible.verbose = "-vvv"
    end
  end

 config.vm.define "ubuntu" do |ubuntu|
    ubuntu.vm.network "private_network", type: "dhcp"
    ubuntu.vm.box = "bento/ubuntu-16.04"
    ubuntu.vm.provision :ansible do |ansible|
        ansible.playbook = "playbook.yml"
        ansible.sudo = "true"
        ansible.sudo_user = "root"
        ansible.host_key_checking = "false"
        ansible.verbose = "-vvv"
    end
 end

end

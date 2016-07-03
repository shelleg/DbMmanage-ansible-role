# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure(2) do |config|
    ansible_verbosity='-v'
    machines = [
      {
        :name => "centos7",
        :box => "centos/7"
      },
      {
        :name => "centos6",
        :box => "geerlingguy/centos6"
      },
      {
        :name => "ubuntu1604",
        :box => "bento/ubuntu-16.04"
      },
      {
        :name => "ubuntu1404",
        :box => "ubuntu/trusty64"
      }
    ]

    machines.each do |opts|

      config.vm.define opts[:name] do |machine|
        machine.vm.hostname = opts[:name]
        machine.vm.box = opts[:box]

        machine.vm.provision :ansible do |ansible|
            ansible.limit = opts[:name]
            ansible.playbook = "./tests/test.yml"
            ansible.sudo = "true"
            ansible.sudo_user = "root"
            ansible.host_key_checking = "false"
            ansible.verbose = "#{ansible_verbosity}"
            ansible.extra_vars = {
              some_var: "some_value",
              dict: {
                kay1: "val1",
                kay2: "val2"
              }
            }
        end
      end
    end
end


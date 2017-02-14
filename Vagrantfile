# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure(2) do |config|

  config.vm.box = "centos/7"

  config.vm.provider :virtualbox do |vb|
    vb.memory = 4096
    vb.cpus = 2
    vb.customize ["modifyvm", :id, "--natdnshostresolver1", "on"]
  end

  config.vm.hostname = "php_mysql"

  config.vm.define :test1qlphpmysql do |host|
    host.vm.hostname = "test1qlphpmysql"
    # NOTE: You can override the boxtype here
    # host.vm.network :private_network, ip: "192.168.2.21"
    host.vm.network "forwarded_port", guest: 80, host: 6080
    host.vm.network "forwarded_port", guest: 443, host: 6443
    host.vm.provider :virtualbox do |v|
      v.customize ["modifyvm", :id, "--name", host.vm.hostname]
    end
  end

  config.vm.provision :ansible do |ansible|
    ansible.groups = {
        "QLFE" => ["php_mysql"],
        "vagrant" => ["php_mysql"]
    }
    ansible.playbook = "playbook.yml"
    ansible.galaxy_role_file = "roles/requirements.yml"
    ansible.galaxy_command = "ansible-galaxy install --role-file=%{role_file} --roles-path=roles/ --ignore-errors"
    ansible.extra_vars = { ansible_winrm_scheme: "http" }
  end
end


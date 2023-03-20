# -*- mode: ruby -*-
# vi: set ft=ruby :

### Configuration parameters in config.yaml file ###
require 'yaml'

current_dir    = File.dirname(File.expand_path(__FILE__))
configs        = YAML.load_file("#{current_dir}/config.yaml")
vagrant_config = configs['configs']

Vagrant.configure("2") do |config|

    ## Ajout de la clés SSH indiquer dans le fichier de configuration
    config.vm.provision "shell", inline: <<-SHELL
        echo #{File.readlines(vagrant_config["pub_key_file"]).first.strip} >> /home/vagrant/.ssh/authorized_keys
    SHELL

    vagrant_config['hosts'].each do |name, ip|
        config.vm.define name do |machine|
            machine.vm.box = vagrant_config['box_url'] 
            machine.vm.hostname = name
            machine.vm.box_url = vagrant_config['box_url'] 
            machine.vm.network :private_network, ip: vagrant_config['ip_suffixe'] + "." + ip
            machine.vm.provider :virtualbox do |v|
                v.customize ["modifyvm", :id, "--natdnshostresolver1", "on"]
                v.customize ["modifyvm", :id, "--natdnsproxy1", "on"]
                v.customize ["modifyvm", :id, "--memory", 512]
                v.customize ["modifyvm", :id, "--name", name]
                v.customize ["modifyvm", :id, "--cpus", "1"]
            end
        end
    end
end
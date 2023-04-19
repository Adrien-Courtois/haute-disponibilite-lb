# -*- mode: ruby -*-
# vi: set ft=ruby :

### Récupération de la liste des Hosts ###
require 'yaml'
hash = YAML.load File.read('ansible/inventory/main.yml')

webservers = hash["all"]["children"]["web_servers"]["children"]
loadbalancers = hash["all"]["children"]["load_balancers_servers"]["children"]
cache = hash["all"]["children"]["cache_servers"]["children"]

Vagrant.configure("2") do |config|

    ## Ajout de la clés SSH indiquer dans le fichier de configuration
    config.vm.provision "shell", inline: <<-SHELL
        echo #{File.readlines("/home/adrien/.ssh/id_rsa.pub").first.strip} >> /home/vagrant/.ssh/authorized_keys
    SHELL

    ## Webservers
    webservers.keys.each do |name|
        config.vm.define name do |machine|
            machine.vm.box = "debian/buster64" 
            machine.vm.hostname = name
            machine.vm.box_url = "debian/buster64" 
            machine.vm.network :private_network, ip: webservers[name]["hosts"]
            machine.vm.provider :virtualbox do |v|
                v.customize ["modifyvm", :id, "--natdnshostresolver1", "on"]
                v.customize ["modifyvm", :id, "--natdnsproxy1", "on"]
                v.customize ["modifyvm", :id, "--memory", 512]
                v.customize ["modifyvm", :id, "--name", name]
                v.customize ["modifyvm", :id, "--cpus", "1"]
            end
        end
    end

    ## Load balancers
    loadbalancers.keys.each do |name|
        config.vm.define name do |machine|
            machine.vm.box = "debian/buster64" 
            machine.vm.hostname = name
            machine.vm.box_url = "debian/buster64" 
            machine.vm.network :private_network, ip: loadbalancers[name]["hosts"]
            machine.vm.provider :virtualbox do |v|
                v.customize ["modifyvm", :id, "--natdnshostresolver1", "on"]
                v.customize ["modifyvm", :id, "--natdnsproxy1", "on"]
                v.customize ["modifyvm", :id, "--memory", 512]
                v.customize ["modifyvm", :id, "--name", name]
                v.customize ["modifyvm", :id, "--cpus", "1"]
            end
        end
    end

    ## cache servers
    cache.keys.each do |name|
        config.vm.define name do |machine|
            machine.vm.box = "debian/buster64" 
            machine.vm.hostname = name
            machine.vm.box_url = "debian/buster64" 
            machine.vm.network :private_network, ip: cache[name]["hosts"]
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
# -*- mode: ruby -*-
# vi: set ft=ruby :

### Récupération de la liste des Hosts ###
# Read the file.
ini = File.read 'ansible/inventory'

# Split the file into sections. Assumes only one blank line between sections
sections = ini.scan /\[.*?\n\n/m

# Return an array, then merge them. Use the first element from each split as the hash key. 
hosts = sections.map do |section| 
  array = section.split
  key   = array.shift.delete '[]'
  { key => array }
end.reduce({}, :merge)

Vagrant.configure("2") do |config|

    ## Ajout de la clés SSH indiquer dans le fichier de configuration
    config.vm.provision "shell", inline: <<-SHELL
        echo #{File.readlines("/home/adrien/.ssh/id_rsa.pub").first.strip} >> /home/vagrant/.ssh/authorized_keys
    SHELL

    ## Webservers
    hosts['web_servers:children'].each do |name|
        config.vm.define name do |machine|
            machine.vm.box = "debian/buster64" 
            machine.vm.hostname = name
            machine.vm.box_url = "debian/buster64" 
            machine.vm.network :private_network, ip: hosts[name][0]
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
    hosts['load_balancers_servers:children'].each do |name|
        config.vm.define name do |machine|
            machine.vm.box = "debian/buster64" 
            machine.vm.hostname = name
            machine.vm.box_url = "debian/buster64" 
            machine.vm.network :private_network, ip: hosts[name][0]
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
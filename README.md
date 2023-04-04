# Haute disponibilité

- [Haute disponibilité](#haute-disponibilité)
  - [Pré-requis](#pré-requis)
    - [Vagrant](#vagrant)
    - [Ansible](#ansible)
  - [Utilisation](#utilisation)
  - [Explications](#explications)
    - [Infrastructure](#infrastructure)
    - [Fichiers configuration](#fichiers-configuration)



## Pré-requis

### Vagrant
L'infrastructure est composée de VM générer par **Vagrant**, voici la [documentation de l'installation](https://developer.hashicorp.com/vagrant/downloads)


### Ansible
Les serveurs sont configurés grâce à **Ansible**, voici la [documentation de l'installation](https://docs.ansible.com/ansible/latest/installation_guide/intro_installation.html)

## Utilisation
Configurer l'infrastructure :
- Configurer les VM dans le fichier `config.yaml`
- Editer l'infrastructure grâce au fichier `ansible/inventory` (qui sert à la fois à Ansible pour configurer les serveurs et aussi à Vagrant pour générer l'infrastructure)

Lancer les VM
```
vagrant up
```

Configurer les serveurs (lance le playbook `ansible/main.yml` qui va faire appel aux autres playbook situés dans le dossier `ansible/playbook`)
```
ansible-playbook -i ansible/inventory -D ansible/main.yml
```

Vous pouvez voir le résultat sur votre navigateur WEB à l'adresse indiquée dans le fichier `ansible/res/keepalived.conf` (par défaut http://192.168.60.100)

## Explications

### Infrastructure

Tout d'abord on créer les VM grâce à **Vagrant**, ensuite on utilise **Ansible** pour installer les paquets et configurer les machines virtuelles. Pour les serveurs WEB on utilse **NGINX** avec un fichier HTML de base indiquant sur quel serveur nous nous trouvons. Côté Load Balancer c'est **HAProxy** qui est utilisé pour rediriger ensuite sur les deux serveurs WEB. Enfin une Virtual IP avec **KeepAlived** pour de la haute disponibilité.

![Illustration de l'infrastructure](img/infrastructure_haute_disponibilité.png)

### Fichiers configuration

- `ansible/res/index.html` : Fichier html afficher sur les serveurs WEB
- `ansible/res/haproxy.cfg` : Fichier de configuration de HAProxy sur les loadbalancers
- `ansible/res/keepalived` : Fichier de configuration de keepalived (virtual IP) sur les loadbalancers
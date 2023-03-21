# Haute disponibilité

## Pré-requis

### Vagrant


### Ansible


## Utilisation
Configurer l'infrastructure :
- Configurer les VM dans le fichier `config.yaml`
- Editer l'infrastructure grâce au fichier `ansible/inventory` (qui sert à la fois à Ansible pour configurer les serveurs et aussi à Vagrant pour générer l'infrastructure)

Lancer les VM
```
vagrant up
```

Configurer les serveurs
```
ansible-playbook -i ansible/inventory -D ansible/playbook.yaml
```

Vous pouvez voir le résultat sur votre navigateur WEB à l'adresse indiquée dans le fichier `ansible/res/keepalived.conf` (par défaut http://192.168.60.100)
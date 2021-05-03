# Formation Terraform Metsys

## 1 - Installation de Terraform et GIT sur WSL

Lancer Windows Terminal, vérifiez bien que vous êtes sur Ubuntu puis exécuter :

`sudo apt-get update && sudo apt-get install -y gnupg software-properties-common curl git-all vim`

> La première partie va mettre à jour votre base de paquets pour connaitre tous les paquets disponibles et la seconde permet d'installer des basics nécessaires pour la suite.

 Ajouter la clé GPG de HashiCorp au gestionnaire de paquets pour valider l'authenticité des paquets téléchargés.

`curl -fsSL https://apt.releases.hashicorp.com/gpg | sudo apt-key add -`

> La première partie télécharge le fichier et envoie le contenu de ce fichier dans l'outil pour ajouter les clés au gestionnaire de paquets.

Ajout du dépôt officiel de HashiCorp dans le gestionnaire de paquets.

`sudo apt-add-repository "deb [arch=amd64] https://apt.releases.hashicorp.com $(lsb_release -cs) main"`

Mise à jour de la base de paquets et installation de Terraform
`sudo apt-get update && sudo apt-get install terraform`

Test de Terraform
`terraform -v`

## Etape suivante
Lorsque ces étapes sont prêtes et que le bogoss de formateur vous autorise à avancer, vous pouvez aller ici:

[Etape 2 - Installation de Azure CLI](https://github.com/HeuScripts/Formation/tree/main/Installation/Etape-2)
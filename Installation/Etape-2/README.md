# Formation Terraform Metsys / Installation

  

## 2 - Installation de Azure CLI

Toujours dans Windows Terminal, lancement d'un petit script d'installation :

`curl -sL https://aka.ms/InstallAzureCLIDeb | sudo bash`

Test de Azure CLI:
`az -v`

Au cas où vous ayez un compte Azure de connecté :
`az lougout`

Connexion à votre compte Azure :
`az login`
> une fenêtre d'authentification s'ouvre sur votre navigateur et vous avez simplement à choisir votre compte Metsys

Ajout automatique des extensions Azure CLI nécessaire sans prompt :
`az config set extension.use_dynamic_install=yes_without_prompt`

Liste de tous les tenants du compte, même ceux avec MFA
`az account tenant list`
> récuperer l'id du tenant créé pour la démo

Si le tenant est protégé par MFA, ajout du tenant :
`az login --tenant <ID_TENANT_AVEC_MFA>`

Récupération de la liste des souscriptions :
`az account list --output table` 
> pour afficher la liste de toutes les souscriptions et récupèrer l'id
> la souscription à utiliser s'appelle "Visual Studio Enterprise Subscription – MPN"

Pour définir la souscription sur laquelle on travaille :
`az account set --subscription "<SUBSCRIPTION_ID>"`

## Etape suivante
Lorsque ces étapes sont prêtes et que le bogoss de formateur vous autorise à avancer, vous pouvez aller ici:

[Etape 3 - Configuration de Visual Studio Code](https://github.com/HeuScripts/Formation/tree/main/Installation/Etape-3)
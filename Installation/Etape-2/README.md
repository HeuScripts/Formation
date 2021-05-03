# Formation Terraform Metsys / Installation

  

## 2 - Installation de Azure CLI

Toujours dans Windows Terminal, lancement d'un petit script d'installation :

```curl -sL https://aka.ms/InstallAzureCLIDeb | sudo bash```

Test de Azure CLI:
```az -v```

Au cas où vous ayez un compte Azure de connecté :
```az lougout```

Connexion à votre compte Azure :
<<<<<<< HEAD
```az login```
=======
`az login`
>>>>>>> 87a5df66be5f91dbb7c67761e7244c3655d04396

>_Explication de la commande :_
>- _az : utiliser des commandes fournies par Azure CLI (ne sera plus détaillée ici)_
>- _login : indiquer que l'on souhaite se connecter à son compte Azure pour utiliser Azure CLI. Pour se déconnecter, utilisez « az logout »._
>- _Plus d'info sur la commande :_ [lien](https://docs.microsoft.com/en-us/cli/azure/reference-index?view=azure-cli-latest#az-login)

Une fenêtre d'authentification s'ouvre sur votre navigateur et vous avez simplement à choisir votre compte Metsys

Ajout automatique des extensions Azure CLI nécessaire sans prompt :
```az config set extension.use_dynamic_install=yes_without_prompt```

>_Explication de la commande :_
>- _config set : définir une valeur de la configuration_
>- _extension.use_dynamic_install=yes_without_prompt : clé=valeur à modifier_
>- _Plus d'info sur la commande : [lien](https://docs.microsoft.com/en-us/cli/azure/config?view=azure-cli-latest#az_config_set)_

<<<<<<< HEAD
Liste de tous les tenants du compte, même ceux avec MFA :
```az account tenant list```
=======
>_Explication de la commande :_
>- _config set : définir une valeur de la configuration_
>- _extension.use_dynamic_install=yes_without_prompt : clé=valeur à modifier_
>- _Plus d'info sur la commande : [lien](https://docs.microsoft.com/en-us/cli/azure/config?view=azure-cli-latest#az_config_set)_

Liste de tous les tenants du compte, même ceux avec MFA :
`az account tenant list`
>>>>>>> 87a5df66be5f91dbb7c67761e7244c3655d04396
> récuperer l'id du tenant créé pour la démo

Si le tenant est protégé par MFA, ajout du tenant :
```az login --tenant <ID_TENANT_AVEC_MFA>```

Récupération de la liste des souscriptions :
<<<<<<< HEAD
```az account list --output table```
=======
`az account list --output table`
>>>>>>> 87a5df66be5f91dbb7c67761e7244c3655d04396

>_Explication de la commande :_
>- _account list : récupérer la liste des abonnements du compte_
>- _--output table : affiche sous forme de tableau_
>- _Plus d'info sur la commande : [lien](https://docs.microsoft.com/en-us/cli/azure/account?view=azure-cli-latest#az-account-list)_

La souscription à utiliser s'appelle "Visual Studio Enterprise Subscription – MPN"

Pour définir la souscription sur laquelle on travaille :
```az account set --subscription "<SUBSCRIPTION_ID>"```

>_Explication de la commande :_
>- _account set : définir l'abonnement du compte_
>- _--subscription "<SUBSCRIPTIONID>" : fournir l'id de l'abonnement à définir par défaut_
>- _Plus d'info sur la commande : [lien](https://docs.microsoft.com/en-us/cli/azure/account?view=azure-cli-latest#az-account-set)_

>_Explication de la commande :_
>- _account set : définir l'abonnement du compte_
>- _--subscription "<SUBSCRIPTIONID>" : fournir l'id de l'abonnement à définir par défaut_
>- _Plus d'info sur la commande : [lien](https://docs.microsoft.com/en-us/cli/azure/account?view=azure-cli-latest#az-account-set)_

## Etape suivante
Lorsque ces étapes sont prêtes et que le bogoss de formateur vous autorise à avancer, vous pouvez aller ici:

[Etape 3 - Configuration de Visual Studio Code](https://github.com/HeuScripts/Formation/tree/main/Installation/Etape-3)
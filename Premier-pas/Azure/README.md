# Formation Terraform Metsys / Premier pas

  

## 3 - Configurer Azure pour travailler avec Terraform

### Récupérer sa SUBSCRIPTION_ID

Pour rappel :

```shell
az account list --query "[].{nom:name, subscriptionid:id}" -o table
```

### Créer un principal de service

> Un principal de service permet de définir des permissions et des stratégies d'accès aux ressources à des services (applications).

Il faut maintenant créer un principale de service sur l'abonnement, que l'on va nommer Terraform pour avoir les droits de créer les ressources sur Azure depuis Terraform.
```shell
az ad sp create-for-rbac --name="terraform" --role="Owner" --scopes="/subscriptions/<SUBSCRIPTIONID>"
```
>_Explication de la commande :_
>- _ad sp : gérer le service principal sur Azure Active Directory pour automatiser l'authentification_
>- _create-for-rbac : créer un principal de service et le configurer pour accéder aux ressources Azure_
>- _--name="terraform" : spécifier le nom du principal de service_
>- _--role="Owner" : définir le rôle du principal de service à Owner pour qu'il puisse créer et gérer ses services sur cet abonnement_
>- _--scopes="/subscriptions/<SUBSCRIPTIONID>" : spécifier l'abonnement sur lequel créer ce principal de service_
>- _Plus d'info sur la commande : [lien](https://docs.microsoft.com/en-us/cli/azure/ad/sp?view=azure-cli-latest#az-ad-sp-create-for-rbac)_

Cette commande va vous retourner des valeurs qu'il va falloir conserver car certaines n'apparaissent qu'une fois (le password).

Nous avons besoin des informations `appID`, `password`, `tenant`.

Note : Si vous perdez le password, relancez la commande qui va mettre à jour le principale de service et générer un nouveau mot de passe.

### Choisir la région Azure

Vous allez devoir choisir une région pour stocker votre fichier d'état, puis une ou plusieurs régions pour le déploiement de vos ressources. Pour connaitre le code de votre région, voici la commande listant les régions disponibles :
```shell
az account list-locations --query 'sort_by([].{Nom:displayName, Code_region:name}, &Nom)' --output table
```
>_Explication de la commande :_
>- _account list-locations: lister les régions disponibles pour l'abonnement_
>- _--query 'sort\_by([].{Nom:displayName, Code\_region:name}, &Nom)' : réduire l'affichage pour n'avoir que 'displayName' et 'name' de toutes les valeurs disponibles ('[].'), mettre des titres aux colonnes avec 'Nom' et 'Code\_region' pour la lisibilité et trier par ordre alphabétique sur 'Nom' (avec &Nom) grâce à 'sort\_by'_
>- _Plus d'info sur la commande : [lien](https://docs.microsoft.com/en-us/cli/azure/account?view=azure-cli-latest#az-account-list-locations)_

Nous avons besoin d'un `code_region`.

### Créer un groupe de ressource pour le fichier d'état Terraform

Comme expliqué plus tôt, le fichier d'état enregistre l'état du dernier déploiement. Pour travailler en équipe ou éviter une perte de ce fichier (panne de disque dur, ordinateur volé…), il est vivement conseillé de créer un fichier sur un support accessible et pérenne.

Nous allons donc créer un compte de stockage sur Azure dans un groupe de ressources dédié à Terraform.

Commençons par la création du groupe de ressources :
```shell
az group create --name <NAME> --location <REGION>
```

En production, un bon nommage est réfléchi et défini en amont mais pour la formation, je vous conseille de choisir des noms courts.
```shell
Par exemple : az group create --name demorg --location europe
```

>_Explication de la commande :_
>- _group create : créer un nouveau groupe de ressources_
>- _--name <NAME> : spécifier le nom du groupe de ressources_
>- _--location <REGION> : spécifier la région dans laquelle créer le groupe de ressources_
>- _Plus d'info sur la commande : [lien](https://docs.microsoft.com/en-us/cli/azure/group?view=azure-cli-latest#az-group-create)_

### Créer le stockage pour Terraform

Nous créons d'abord le compte de stockage sur Azure puis nous créerons un conteneur.
```shell
az storage account create --name <NAME> --resource-group <RESOURCEGROUP> --location <REGION>
```

>_Explication de la commande :_
>- _storage account create : créer un compte de stockage_
>- _--name <NAME> : spécifier le nom du compte de stockage_
>- _--resource-group <RESOURCEGROUP> --location <REGION> : specifier le groupe de ressources et la région_
>- _--sku <SKU> : le SKU par défaut est Standard\_RARGS mais vous pouvez spécifier un SKU autre à partir de [cette liste](https://docs.microsoft.com/en-us/rest/api/storagerp/srp_sku_types))_
>- _Plus d'info sur la commande : [lien](https://docs.microsoft.com/en-us/cli/azure/storage/account?view=azure-cli-latest#az-storage-account-create)_

Avec ce nouveau compte de stockage (<ACCOUNTNAME>), nous pouvons créer un conteneur à l'intérieur.
```shell
az storage container create --name <NAME> --account-name <ACCOUNTNAME> --public-access container --resource-group <RESOURCEGROUP>
```
>_Explication de la commande :_
>- _storage container create : créer un conteneur dans le compte de stockage_
>- _--name <NAME> : spécifier le nom du conteneur_
>- _--account-name <ACCOUNTNAME> : spécifier le compte de stockage sur lequel créer le conteneur_
>- _--public-access container : rend le contenu accessible publiquement_
>- _--resource-group <RESOURCEGROUP> --location <REGION> : spécifier le groupe de ressources et la région_
>- _Plus d'info sur la commande : [lien](https://docs.microsoft.com/en-us/cli/azure/storage/container?view=azure-cli-latest#az-storage-container-create)_

Le conteneur est créer, il faut maintenant pouvoir récupérer les identifiants pour se connecter dessus.
```shell
az storage account show-connection-string --name <ACCOUNTNAME> --resource-group <RESOURCEGROUP>
```
>_Explication de la commande :_
>- _storage container show-connection-string : récupérer les chaines de connexion du compte de stockage_
>- _--name <ACCOUNTNAME> : spécifier le nom du compte de stockage_
>- _--resource-group <RESOURCEGROUP> : spécifier le groupe de ressources_
>- _Plus d'info sur la commande : [lien](https://docs.microsoft.com/en-us/cli/azure/storage/account?view=azure-cli-latest#az-storage-account-show-connection-string)_

Récupérer et garder la sortie de la commande, nous en aurons besoin pour connecter Terraform.

### Définir des valeurs par défaut

Comme vous avez pu le constater, il faut saisir à chaque commande le `resource-group`. Pour gagner du temps par la suite, vous pouvez définir des valeurs par défaut :
```shell
az configure --defaults location=<REGION> group=<RESOURCEGROUP>
```
>_Explication de la commande :_
>- _configure --defaults : permettre de spécifier des valeurs par défaut_
>- _location=<REGION> resource-group <RESOURCEGROUP> : spécifier la région et le groupe de ressources_
>- _Plus d'info sur la commande : [lien](https://docs.microsoft.com/fr-fr/cli/azure/azure-cli-configuration#cli-configuration-with-az-configure)_

J'aurai pu vous le dire plus tôt...

## Etape suivante
La suite est ici:

[4 - Utiliser GIT (un peu)](https://github.com/HeuScripts/Formation/tree/main/Premier-pas/Git/)
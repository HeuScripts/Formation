# Formation Terraform Metsys / Premier pas

  

## 5 - Configurer Terraform et déployer sa première infra

Le stockage est prêt, tout est installé, vous avez même vos premiers fichiers Terraform.

Les deux premiers blocs du fichier provider.tf contiennent la description du backend Terraform et le provider. Nous aurions pu faire deux fichiers séparés, un pour le backend et un pour lister tous les providers mais comme Terraform ne fait pas de distinction, nous avons trouvé plus pratique de grouper ces deux blocs.

La définition du backend sert à décrire où est stocké le fichier d'état de Terraform
```hcl
terraform {
  backend "azurerm" {
    resource_group_name = "<RESOURCEGROUP>"
    storage_account_name = "<ACCOUNTNAME>"
    container_name = "<CONTAINERNAME>"
    key = "<PROJECTNAME>.tfstate"
  }
}
```
Il faut donc fournir le compte de stockage avec le groupe de ressource dans lequel il est, le conteneur qui stocke le fichier d'état le nom du fichier d'état. Si vous décidez d'utiliser ce conteneur pour plusieurs projets, choisissez un nom de fichier d'état ('key') avec un nom distinct.

Le second bloque dit simplement à Terraform de charger ce qu'il faut pour communiquer avec Azure RM et de figer la version du provider à 2.24. Le numéro de version est facultatif mais permet de fixer la version sur votre projet pour vous assurer que votre projet fonctionne malgré les évolutions du provider. Vous pouvez changer la version du provider en la récupérant sur la page GitHub du projet du provider (ici https://github.com/terraform-providers/terraform-provider-azurerm).
```hcl
provider "azurerm" {
  features {}
}
```
### Initialisation de votre projet avec Terraform

Vos fichiers sont configurés avec vos informations, Terraform a tout ce qu'il faut pour démarrer. Initialisez le projet pour charger l'extension du provider.
```shell
cd <WORKINGDIR>/<PROJECT>
terraform init
```
> Vous constaterez qu'un dossier .terraform a été créé. Il contient l'extension "azurerm" et un fichier d'état.

### Création de la planification

Terraform est initialisé, il faut maintenant planifier les déploiements.
```shell
cd <WORKINGDIR>/<PROJECT>
terraform plan
```
Là, comme nous n'avons rien mis dans nos fichiers, il n'y a évidemment rien à planifier :

> No changes. Infrastructure is up-to-date.

Ajoutons des ressources.

### Création de ressources

Pour commencer simplement, ouvrez le fichier main.tf présent dans la démo et décommentez le premier bloc.
```hcl
resource "azurerm_resource_group" "example" {
  name     = "formationterraformrg"
  location = "<LOCATION>"
}
```
Ici, on créé une `resource` de type `azurerm_resource_group` que l'on nomme `example` à travers Terraform. Le fait de nommer la ressource permet de s'en resservir à travers Terraform (par exemple, pour fournir le nom du groupe de ressource sur une machine virtuelle). Nous reviendrons dessus plus tard.

En paramètre de ce bloc, on indique le nom du groupe de ressource (ici 'formationterraformrg') et sa localisation qui doit être une localisation Azure.

_Note : sur chaque ressource utilisée dans le dossier de démo, un lien vers la documentation de la ressource est ajouté en commentaire._

### Déploiement des ressources

Vous avez une première ressource de déclarée dans vos fichiers (un groupe de ressources). Utilisons Terraform pour déployer cette ressource Terraform. Comme précédemment, allez dans votre répertoire de travail et lancez la planification, puis déployez (apply).
```shell
cd <WORKINGDIR>/<PROJECT>
terraform plan
terraform apply
```
On constate qu'à l'étape de planification, la liste des changements apparait avec des symboles pour indiquer si la ressource est ajouté, modifié ou supprimé. Un résumé totalise les différents changements.

Lorsque l'on déploie avec 'apply', toute l'étape de planification se fait à nouveau et une question est posée pour valider le déploiement. Pour éviter ça, il faut stocker la planification dans un fichier (avec l'argument `--out run.tfplan`) et la fournir comme plan pour le déploiement.

Ce qui donne :
```hcl
cd <WORKINGDIR>/<PROJECT>
terraform plan --out run.tfplan
terraform apply run.tfplan
```
Voilà, vous avez déployé une ressource sur Azure grâce à Terraform. Bien joué !

Connectez-vous sur Azure pour aller voir ce nouveau groupe de ressources.

### Mise à jour des ressources

Dans le fichier main.tf, décommentez la seconde partie (tout le reste du fichier). Dans cet exemple, vous allez déployer un machine virtuelle.

Comme précédemment, il faut planifier le déploiement et déployer.
```shell
cd <WORKINGDIR>/<PROJECT>
terraform plan --out run.tfplan
terraform apply run.tfplan
```
Le déploiement peut durer quelques minutes, c'est normal.

Une fois terminé, vous pouvez constater sur Azure que le groupe de ressources précédemment créé n'est pas modifié, seule la seconde partie du code est déployé (le réseau et sous-réseau, l'interface réseau et la machine virtuelle). 

C'est tout l'intérêt de faire du déclaratif ! Terraform va essayer d'arriver à l'état demandé en tenant compte de ce qui existe déjà, il ne va pas déployer à nouveau le groupe de ressource.

D'ailleurs, si vous essayez à nouveau les commandes précédentes, Terraform vous dira qu'il n'y a rien à faire.

### Destruction des ressources

Maintenant que nous avons pu installer le nécessaire, vu le fonctionnement de Terraform et déployé une petite infrastructure de démo, nous pouvons tout détruire.

Le processus est similaire, on planifie la destruction grâce à l'argument `--destroy` et on la déploie.

```shell
cd <WORKINGDIR>/<PROJECT>
terraform plan --out run.tfplan --destroy
terraform apply run.tfplan
```

Comme Terraform connait votre infra et ce qu'il a déployé, il ne détruira que ça.

## Etape suivante
La suite est ici:

[5 - Configurer Terraform et déployer sa première infra](https://github.com/HeuScripts/Formation/tree/main/Premier-pas/Terraform)
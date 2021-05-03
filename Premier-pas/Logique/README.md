# Formation Terraform Metsys / Premier pas

  

## 1 - Logique

Terraform n'a pas besoin d'être installé sur Azure, c'est un outil local (relatif à votre shell bien qu'il puisse être utilisé à travers CloudShell) qui interprète des fichiers YAML pour communiquer avec des fournisseurs (Providers). Azure est un provider mais Terraform peut communiquer avec de nombreux autres providers [officiels](https://www.terraform.io/docs/providers/index.html) et [communautaires](https://www.terraform.io/docs/providers/type/community-index.html)). En fait, Terraform peut communiquer avec tous les services disposant d'une API pour être managés mais aussi d'autres providers locaux (fichiers, générateur de suites aléatoires, heure…).

Terraform créé un état de l'infrastructure dans un fichier. Lorsque vous souhaitez ajouter des ressources, Terraform va calculer la différence entre l'état actuel et l'état souhaité et pourra appliquer les changements pour arriver à l'état souhaité. A l'inverse, pour supprimer ou modifier des ressources, le processus est identique. Attention, si des modifications sont apportées en dehors de Terraform (par exemple à travers le portail Azure), tout sera supprimé lors de la prochaine application de votre configuration si vous n'avez pas intégré les modifications dans vos fichiers Terraform.

Lorsque vous travaillez avec Terraform, il faut donc bien garder en tête que ce n'est pas un outil pour créer des ressources n'importe où mais un outil pour créer, maintenir et supprimer une seule infrastructure à la fois. Si vous souhaitez travailler sur plusieurs projets, plusieurs clients, plusieurs environnements qui n'ont pas de liens entre eux, il faut donc créer un fichier d'état par projet/infra/client.

Le fichier d'état `.tfstate` peut être stocké localement sur votre poste mais dans ce cas, vous seul avez la possibilité de maintenir l'infrastructure avec Terraform. C'est acceptable en développement mais pas en production. Il faut donc rendre le fichier d'état accessible sur une ressource partagée comme un compte de stockage Azure.

Une fois les fichiers de configuration de votre projet prêts, le processus est simple :

- Vous initialisez pour qu'il détecte tous les fichiers du répertoire et télécharge les connecteurs liés aux providers déclarés (les connecteurs sont des exécutables permettant de dialoguer avec les providers)
- Vous planifiez le déploiement (ou la destruction) en chargeant si besoin des paramètres supplémentaires. Cette planification génère un fichier « plan » qui reprend tout ce qui est à faire par rapport à l'état actuel.
- Vous appliquez le plan

Les fichiers de votre projet sont lus de façon arbitraire par Terraform. Il n'y a pas de convention pour spécifier les premiers fichiers à lire, ni comment nommer les fichiers à part leur extension : `.tf`.

Il faut donc prévoir les dépendances entre les ressources directement dans les fichiers.

Il est aussi possible d'avoir des fichiers contenant des variables spécifiques (par exemple des mots de passe que vous ne souhaitez pas rendre publiques sur GitHub ou dans des processus d'évolution d'une infrastructure, le type d'environnement).

Néanmoins, les bonnes pratiques conseils d'avoir :

- `main.tf` pour décrire le projet
- `output.tf` pour récupérer les sorties des ressources
- `providers.tf` pour déclarer les providers
- `version.tf` pour fixer la version de Terraform ou (pour des raisons de compatibilité)
- `variables.tf` pour déclarer les variables utilisées

Il est possible de créer des modules dans Terraform pour découper les projets importants en petites parties. Ces modules peuvent aussi être réutilisés ailleurs sur d'autres projets mais en aucun cas il faut les imaginer comme des librairies de modules.

## Etape suivante
Lorsque ces étapes sont prêtes et que le bogoss de formateur vous autorise à avancer, vous pouvez aller ici:

[Etape 3 - Configuration de Visual Studio Code](https://github.com/HeuScripts/Formation/tree/main/Premier-Pas/Preambule/)
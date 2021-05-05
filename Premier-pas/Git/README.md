# Formation Terraform Metsys / Premier pas

  

## 4 - Utiliser GIT (les bases)



Comme vous n'aurez pas à modifier les fichiers sur GitHub pour le moment, nous allons voir tout ce qui permet de simplement récupérer des fichiers d'un dépôt GIT (repository).

Rendez-vous sur https://github.com/HeuScripts/Demo-terraform et copier le lien pour "cloner" le répertoire :

<img src="https://github.com/HeuScripts/Formation/blob/main/Premier-pas/Git/git1.jpg" alt="Git clone from GitHub" />

Une fois le lien récupéré, revenez sur votre terminal Linux et placez vous dans votre HOME :

```shell
cd ~
```

Créez un dossier pour la formation :

```shell
mkdir formation
```

Allez dans ce dossier :

```shell
cd formation
```

Récupérez le contenu de la démo terraform depuis GitHub :

```shell
git clone https://github.com/HeuScripts/Demo-terraform.git
```

> En fait, vous n'aviez pas besoin de copier le lien sur GitHub, c'était juste pour comprendre la démarche mais bon, c'est trop tard.

Ca y est, vous avez vos premiers fichiers Terraform !


## Etape suivante
La suite est ici:

[5 - Configurer Terraform et déployer sa première infra](https://github.com/HeuScripts/Formation/tree/main/Premier-pas/Terraform)
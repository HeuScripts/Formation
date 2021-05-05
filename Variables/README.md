# Formation Terraform Metsys / Premier pas

  

## 1 - Variables et noms de ressources

### Noms de ressources

Vous l'aurez peut-être constaté mais dans notre `main.tf`, il a fallu mettre 4x la localisation et 5x fois le nom du groupe de ressource.

En soit, rien de bien grave sauf si vous décidez d'utiliser ce `main.tf`, qu'il devient bien plus conséquent en terme de ressource et que vous décidez de changer le nom du groupe de ressources.

Pour faire autrement, il est possible d'appeler une ressource Terraform sous la forme : `typedelaressource.nom.attribut`.

Par exemple, vous trouverez dans notre `main.tf` :
```hcl
resource "azurerm_virtual_network" "example" {
  name                = "vnet-example-fc-01"
  address_space       = ["10.0.0.0/16"]
  location            = "<LOCATION>"
  resource_group_name = "formationterraformrg"
}

resource "azurerm_subnet" "internal" {
  name                 = "snet-example-fc-01"
  resource_group_name  = "formationterraformrg"
  virtual_network_name = azurerm_virtual_network.example.name
  address_prefix       = "10.0.2.0/24"
}
```

La partie `azurerm_virtual_network.example.name` fait donc référence à l'attribut `name` de la ressource de type `azurerm_virtual_network` nommée `example`.

A votre tour de modifier le `main.tf` pour utiliser au maximum ces attributs. Pour valider que ça fonctionne, le mieux reste de déployer !

### Variables

Les variables permettent de définir des valeurs arbitraires pour s'en servir plus tard.

Par exemple :`

```hcl
variable "user_information" {
  type = object({
    name    = string
    address = string
  })
}

resource "some_resource" "example" {
  name    = var.user_information.name
  address = var.user_information.address
}
```
Ca peut être utile pour définir ce qui peut changer comme le nombre de machine à déployer, le nom de l'administrateur...

A votre tour de créer une variable dans notre `main.tf` qui stockerait le mot de passe administrateur de la machine virtuelle.

### Autres

Il y a plein d'autres subtilités possibles sur Terraform, comme `output`, `depends_on`, `count`, `for_each`... et des providers pour faire ajouter des fonctionnalités (l'heure, les nombres aléatoires, le chiffrement, l'écriture de fichier...).

Plutôt que de tous les passer, je vous invite à chercher dans la documentation au fur et à mesure de votre développement.

## On attaque !

Maintenant, c'est chacun pour sa peau !
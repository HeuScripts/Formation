# Formation Terraform Metsys

## 1 - Installation de Terraform, GIT
`$ sudo apt-get update && sudo apt-get install -y gnupg software-properties-common curl git-all vim`
Add the HashiCorp GPG key.
`$ curl -fsSL https://apt.releases.hashicorp.com/gpg | sudo apt-key add -`
Add the official HashiCorp Linux repository.
`$ sudo apt-add-repository "deb [arch=amd64] https://apt.releases.hashicorp.com $(lsb_release -cs) main"`
Update to add the repository, and install the Terraform CLI.
`$ sudo apt-get update && sudo apt-get install terraform`

Plugin VScode

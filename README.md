# Azure-Terraform-Edouard-Ahmed

## 1. Structure du projet
```
├── deployement.yml
├── inventory.ini
├── main.tf
├── modules
│   └── network
│       ├── main.tf
│       └── variables.tf
├── nsg.tf
├── output.tf
├── playbook.yml
├── provider.tf
├── README.md
├── terraform.tfstate
├── terraform.tfstate.backup
└── variables.tf
```

## 2. Description des fichiers principaux
### 2.1 Fichiers Terraform

* **main.cf** — Décrit les ressources principales à déployer mais aussi l'adhération de la vault, la création des VM's, donner une IP publique et privé aux VM's qui sont elle dans 2 Réseaux différent.
* **provider.tf** — Configure le fournisseur (Azure).
* **variables.tf** — Définit les variables d’entrée utilisées dans les scripts par exemple la localisation sur azure eastus pour notre cas.
* **output.tf** — Affichage des IP Publique des VM's.
* **nsg.tf** — Crée et configure les Network Security Groups, ça ouvre le port 22 ainsi que la range de 30000-35000 pour que l'application nginx deployer sur kubernetes soit accessible.
* **terraform.tfstate / terraform.tfstate.backup** — Fichiers d’état de Terraform.

### 2.2 Module réseau
* **modules/network/main.tf** — Ressources réseau (VNet, sous-réseaux, etc.).
* **modules/network/variables.tf** — Variables spécifiques au module réseau.

### 2.3 Fichiers Ansible
* **inventory.ini** — liste des deux VM's deployer sur Azure.
* **playbook.yml** — Tâches Ansible pour installer K3s sur les VM's déployer ainsi que l'initialisation du cluster.
* **deployment.yml** — Déploiement de l'application Nginx.

### 2.4 Vault
Ajout d'une Vault en local pour le stockage des secrets, dans notre cas nous stockant uniquement le username qui nous permet d'accèder au VM's.

### 2.5 Déploiement de l'application
Pour déployer l'application dans un premier temps nous éxécutant,
### 2.5 Déploiement de l'application  

Pour déployer l’application, on suit les étapes classiques de Terraform puis celle de Ansible :  

1. **Initialisation du projet**  
   Prépare l’environnement et télécharge les modules nécessaires :  
   ```sh
   $ terraform init
   ```

2. **Planification du déploiement**  
   Affiche en détail les ressources qui seront créées ou modifiées :  
   ```sh
   $ terraform plan
   ```
   
3. **Application du plan**  
   Lance réellement le déploiement sur Azure :  
   ```sh
   $ terraform apply
   ```

4. **Installation de K3s**  
   Installe K3s sur les Vm's :  
   ```sh
   $ ansible-playbook -i inventory.ini playbook.yml
   ```
   
5. **Déploiement Nginx**  
   Déploie Nginx sur le Cluster Kubernetes :  
   ```sh
   $ ansible-playbook -i inventory.ini deployement.yml 
   ```
   
## 3. Auteurs
* **Édouard Stamboulian** — [GitHub de Édouard](https://github.com/estamboulian)  
* **Ahmed Khairi** — [GitHub de Ahmed](https://github.com/hirlho)  


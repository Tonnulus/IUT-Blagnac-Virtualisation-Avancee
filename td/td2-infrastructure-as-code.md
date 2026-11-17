# TD 2 : Infrastructure as Code

## Exercices théoriques

Les exercices théoriques disponibles sur Moodle doivent être terminés avant la prochaine séance.

Pour les réaliser, vous pouvez vous appuyer sur les pages suivantes ainsi que sur les cours de cette année et de l'année précédente :

- [Red Hat : qu'est-ce que le DevOps ?](https://www.redhat.com/fr/topics/devops)
- [IBM : qu'est-ce que le DevOps ?](https://www.ibm.com/fr-fr/topics/devops)
- [IBM : IaaS, PaaS et SaaS](https://www.ibm.com/topics/iaas-paas-saas)
- [OVHcloud](https://www.ovhcloud.com)
- [LeMagIT : définition de l'Infrastructure as Code](https://www.lemagit.fr/definition/Infrastructure-as-a-Code)

La suite de la séance se déroule en autonomie, sur les ordinateurs de l'IUT, sous la partition Ubuntu 22.04. N'oubliez pas de prendre des notes et de faire des captures d'écran pour rédiger votre compte rendu individuel. Je vous recommande d'utiliser la commande `script` pour récupérer, en fin de séance, tout ce qui s'est affiché dans votre terminal.

Commencez par définir la variable `MonNom`, utilisée dans tous les chemins de ce TD (à refaire dans chaque nouveau terminal), puis lancez l'enregistrement avec `script` :

```bash
$ export MonNom=dupont   # votre nom, en minuscules, sans espace
$ mkdir -p ~/R5A09/"${MonNom:?}" && cd $_ && script seance2.txt
[... Commandes de la séance ...]
$ exit
```

Le compte rendu devra présenter votre progression sur les quatre séances ainsi que vos réponses aux questions de l'énoncé.

Pendant le TP, vous allez télécharger des images depuis Docker Hub. Il est possible que vous atteigniez la limite de téléchargements (*rate limit*), qui vous empêche alors de télécharger de nouvelles images. Dans ce cas, créez un compte sur [Docker Hub](https://hub.docker.com), puis, dans **Account Settings → Security**, ajoutez un nouveau jeton d'accès (*Access Token*).

Connectez-vous ensuite depuis votre poste avec la commande suivante :

```bash
$ docker login
```

## Exercice pratique 1 : Ansible

Ansible est un outil open source développé par Red Hat. Il permet d'automatiser la configuration de vos serveurs : ses modules sont déclaratifs et idempotents (chacun décrit l'état souhaité d'un élément du système, et Ansible n'agit que si l'état réel en diffère), et les tâches sont exécutées dans l'ordre du playbook. L'un de ses avantages est qu'il n'a besoin d'aucun agent sur les machines cibles pour les configurer : tout passe par SSH. Son autre avantage découle de cette idempotence : contrairement à la plupart des scripts, le résultat d'Ansible est prévisible et reproductible, même si on l'exécute plusieurs fois.

**Configuration de votre poste Linux**

SSH (*Secure Shell*) est un protocole de communication sécurisé qui permet d'accéder à un serveur distant.

Vérifiez qu'un serveur SSH est bien à l'écoute sur votre PC :

```bash
$ systemctl status ssh
```

Si le service n'existe pas, installez OpenSSH :

```bash
$ sudo apt-get update
$ sudo apt-get install openssh-server
```

Vérifiez que vous disposez d'une clé SSH :

```bash
$ ls -a ~/.ssh/
```

Si le répertoire est vide, générez une clé SSH :

```bash
$ ssh-keygen
```

Cette commande génère deux fichiers :

- `id_rsa` : votre clé privée, qui ne doit jamais être communiquée et qui peut être chiffrée par une phrase de passe (*passphrase*) ;
- `id_rsa.pub` : votre clé publique, à déposer sur les serveurs distants auxquels vous voulez vous connecter.

Copiez votre clé publique sur votre propre PC avec la commande suivante :

```bash
$ ssh-copy-id localhost
```

Si votre voisin l'accepte, copiez aussi votre clé sur son PC (remplacez `${IP_Voisin}` par l'adresse IP de son poste, ou définissez cette variable avec `export`) :

```bash
$ ssh-copy-id ${IP_Voisin}
```

Testez la connexion SSH en local, puis vers l'adresse IP de votre voisin :

```bash
$ ssh ${IP_Voisin}
```

Installez Ansible sur votre poste, ainsi que la bibliothèque Python Docker (`python3-docker`), dont les modules Docker d'Ansible (`docker_container`, `docker_network`…) ont besoin :

```bash
$ sudo apt-get update
$ sudo apt-get install ansible python3-docker
```

Vérifiez l'installation :

```bash
$ ansible --version
```

Ubuntu 22.04 fournit Ansible 2.10, une version ancienne : certains modules présentés dans la documentation en ligne, comme `docker_container_exec`, n'y existent pas encore. C'est pourquoi les playbooks de ce TD exécutent les commandes dans les conteneurs avec `docker exec`.

Ansible s'appuie sur un inventaire (*inventory*) pour savoir sur quels hôtes cibles exécuter les tâches qui lui sont confiées.

Créez un fichier d'inventaire :

```bash
$ mkdir -p ~/R5A09/"${MonNom:?}"/ansible && cd $_ && vi hosts
```

Remplacez `${IP_Voisin}` par l'adresse IP du poste de votre voisin :

```ini
[my_group_host]
localhost
${IP_Voisin}
```

Ansible pourra ainsi exécuter ses playbooks sur votre poste pour y appliquer ses configurations.

Testez la connectivité :

```bash
$ ansible all -m ping -i hosts
```

**Question 1.1 :** quel résultat obtenez-vous ?

Exécutez la commande suivante (le shell remplace `$HOME` et `${MonNom}` par leur valeur) :

```bash
$ ansible localhost -m file \
    -a "path=$HOME/R5A09/${MonNom}/adhoc state=directory"
```

**Question 1.2 :** quel résultat obtenez-vous ?

La commande précédente exécute le module `file` sur la machine ciblée (ici `localhost`). Un module est une unité de travail qui accomplit une tâche précise. Ansible fournit de nombreux modules de base, mais il est aussi possible de créer les siens.

À l'aide de la [documentation du module file](https://docs.ansible.com/ansible/latest/collections/ansible/builtin/file_module.html), modifiez la commande précédente pour restreindre les droits sur ce répertoire en retirant tout accès aux autres utilisateurs (*others*).

**Question 1.3 :** quelle est la nouvelle commande à exécuter ?

Jusqu'à présent, nous avons utilisé la commande `ansible` avec l'option `-m` pour appeler un module précis. Or la configuration d'un serveur peut faire appel à des dizaines, voire des centaines de modules. C'est pourquoi on utilise des playbooks. Un playbook est un fichier YAML qui décrit les tâches, c'est-à-dire les étapes à suivre pour automatiser un processus. Ansible s'appuie sur l'inventaire pour exécuter les tâches d'un playbook sur les hôtes cibles.

Lors de l'exécution d'un playbook, les tâches s'exécutent l'une après l'autre. La première, « Gathering Facts », n'est pas décrite explicitement dans le playbook : Ansible exécute des commandes pour collecter des informations de base sur les hôtes cibles (les *facts*), afin de pouvoir adapter les tâches et les playbooks à ces données. La commande suivante liste les facts collectés par Ansible :

```bash
$ ansible -m setup all -i hosts
```

**Question 1.4 :** donnez deux exemples d'informations utiles contenues dans les facts.

Nous allons maintenant créer notre premier playbook, pour automatiser l'installation d'un serveur web avec Docker.

Copiez le contenu suivant dans le fichier `~/R5A09/${MonNom}/ansible/playbook-nginx.yml` :

```yaml
---
- name: Déploiement d'un Serveur Web Nginx avec Docker et Volume
  hosts: localhost
  become: yes
  tasks:
    - name: Installation de Docker
      apt:
        name: docker-ce
        state: present
      when: ansible_os_family == 'Debian'
    - name: Installation de Docker
      yum:
        name: docker-ce
        state: present
      when: ansible_os_family == 'RedHat'
    - name: Démarrage du service Docker
      service:
        name: docker
        state: started
    - name: Création du répertoire pour la page web
      file:
        path: /opt/nginx_html
        state: directory
    - name: Copie du fichier template HTML
      template:
        src: helloworld.html.j2
        dest: /opt/nginx_html/index.html
    - name: Lancement du conteneur Nginx avec volume
      become: no # conteneur lancé avec votre compte
      docker_container:
        name: nginx_web_server
        image: nginx
        ports:
          - "80:80"
        volumes:
          - /opt/nginx_html:/usr/share/nginx/html
        restart_policy: always
```

La directive `become: yes` indique à Ansible d'exécuter les tâches avec les droits d'administrateur (via `sudo`) : c'est nécessaire pour installer des paquets ou écrire dans `/opt`. La dernière tâche la désactive (`become: no`) : le conteneur est créé avec votre compte, comme vos commandes `docker`, et profite donc de votre éventuel `docker login`.

Créez ensuite le fichier `~/R5A09/${MonNom}/ansible/helloworld.html.j2`, qui simule un site web. Jinja2 est le moteur de modèles (*templates*) qu'utilise Ansible pour générer des fichiers de configuration ou des données dynamiques pendant l'exécution des tâches d'un playbook. Un modèle peut contenir des variables, des conditions et des boucles. Ici, il utilise la variable `ansible_hostname`, définie dans les facts.

```jinja
<html>
<body>
<h1> Hello ! you are on {{ ansible_hostname }}</h1>
</body>
</html>
```

Exécutez le playbook avec l'option `-K` (`--ask-become-pass`), qui vous demande votre mot de passe (invite `BECOME password:`) pour qu'Ansible puisse utiliser `sudo`. Utilisez cette option pour chaque playbook qui contient `become: yes` : sans elle, le playbook échoue avec une erreur mentionnant `sudo: a password is required`.

```bash
$ cd ~/R5A09/${MonNom}/ansible/ && ansible-playbook -K playbook-nginx.yml
```

Les avertissements `No inventory was parsed` (les playbooks ciblent `localhost`, qui n'a pas besoin d'inventaire) et `DEPRECATION WARNING` (à propos de l'option `container_default_behavior`) peuvent être ignorés.

**Question 1.5 :** pourquoi toutes les tâches ne sont-elles pas à l'état « changed » ?

Dans votre navigateur, rendez-vous sur <http://localhost>.

**Question 1.6 :** quel résultat obtenez-vous ?

Il est préférable d'écrire des playbooks génériques, pour pouvoir les réutiliser. Nous allons définir des variables pour des informations comme le port du serveur web.

Créez, à côté du playbook, un fichier YAML `vars.yml` contenant les variables suivantes (vous le compléterez plus loin, pour WordPress) :

```yaml
web_port: 8080
app_version: 1.0
```

Modifiez le playbook pour qu'il charge ces variables :

```yaml
[...]
  become: yes
  vars_files:
    - vars.yml
  tasks:
[...]
```

Faites en sorte que le conteneur `nginx_web_server` soit exposé sur le port défini dans `vars.yml` : dans la section `ports`, remplacez le port de l'hôte, c'est-à-dire le premier `80`, par `{{ web_port }}`. Relancez le playbook (toujours avec `-K`) : le site est désormais accessible sur <http://localhost:8080>.

**Question 1.7 :** comment afficher sur notre site la version indiquée dans le fichier `vars.yml` ? Montrez le résultat.

Dans certains cas, il est utile de recourir à des conditions. Créez un playbook, par exemple `create_directory_with_condition.yml`, qui utilise la date courante pour créer un répertoire, après avoir vérifié qu'il n'existe pas déjà. Il n'écrit que dans `/tmp` : il n'a donc pas besoin de `become`, ni de l'option `-K`.

```yaml
---
- name: Création d'un Répertoire Dynamique avec Condition
  hosts: localhost # Changez l'hôte en fonction de votre inventaire

  tasks:
    - name: Obtenir la date actuelle
      command: date +"%Y%m%d%H%M%S"
      register: current_date
      changed_when: false # Indique que cette tâche ne modifie pas l'état

    - name: Vérifie si le répertoire existe déjà
      stat:
        path: "/tmp/my_dir"
      register: directory_check
      ignore_errors: yes

    - name: Crée un répertoire basé sur la date actuelle (si n'existe pas)
      file:
        path: "/tmp/my_dir/{{ current_date.stdout }}"
        state: directory
      when: not directory_check.stat.exists
```

Exécutez ce playbook deux fois :

```bash
$ ansible-playbook create_directory_with_condition.yml
```

**Question 1.8 :** quels résultats donne l'exécution de ce playbook ? Expliquez ce comportement.

Il est fréquent de devoir répéter plusieurs fois la même tâche. Plutôt que de copier-coller le bloc de code, on peut utiliser des boucles.

Créez un fichier YAML `users.yml` contenant la liste des noms des utilisateurs à créer sur la machine cible :

```yaml
users:
  - user1
  - user2
  - user3
  - user4
```

Créez un playbook, par exemple `create_users.yml`, qui crée ces utilisateurs sur la machine cible à l'aide d'une boucle sur cette liste :

```yaml
---
- name: Création d'Utilisateurs avec Boucle
  hosts: localhost # Changez l'hôte en fonction de votre inventaire
  become: yes

  tasks:
    - name: Inclure la liste de noms d'utilisateurs
      include_vars:
        file: users.yml

    - name: Crée les utilisateurs
      user:
        name: "{{ item }}"
        state: present
      with_items: "{{ users }}"
```

Ce playbook modifie les comptes du système : exécutez-le avec `-K`.

```bash
$ ansible-playbook -K create_users.yml
```

**Question 1.9 :** quel résultat obtenez-vous ?

**Question 1.10 :** comment supprimer ces utilisateurs ?

Modifiez ce playbook pour supprimer les utilisateurs que vous avez créés, ainsi que leur répertoire personnel.

Comme lors du TD précédent, nous allons installer WordPress avec Docker, mais cette fois à l'aide d'Ansible et des images officielles `mariadb` et `wordpress`. Ce playbook ne manipule que des conteneurs : il n'a pas besoin de `become`. Copiez le contenu suivant dans le fichier `wordpress.yml` :

```yaml
---
- name: Install WordPress
  hosts: localhost
  tasks:

    - name: Create a network
      docker_network:
        name: network_wordpress

    - name: Lance MariaDB
      docker_container:
        name: mariadb
        image: mariadb:11.4
        state: started
        volumes:
          - mariadb_data:/var/lib/mysql
        env:
          MARIADB_ROOT_PASSWORD: "Root_password_top_secret"
          MARIADB_DATABASE: "wordpress"
          MARIADB_USER: "wordpress"
          MARIADB_PASSWORD: "Password_top_secret"
        networks:
          - name: network_wordpress

    - name: Attend que MariaDB soit prête
      command: >-
        docker exec mariadb
        healthcheck.sh --connect --innodb_initialized
      register: mariadb_health
      until: mariadb_health.rc == 0
      retries: 12
      delay: 5
      changed_when: false

    - name: Lance WordPress
      docker_container:
        name: wordpress
        image: wordpress:6
        state: started
        ports:
          - "80:80"
        volumes:
          - wordpress_data:/var/www/html
        env:
          WORDPRESS_DB_HOST: "mariadb"
          WORDPRESS_DB_NAME: "wordpress"
          WORDPRESS_DB_USER: "wordpress"
          WORDPRESS_DB_PASSWORD: "Password_top_secret"
        networks:
          - name: network_wordpress
```

Le port de MariaDB n'est pas publié sur l'hôte : seul WordPress, branché sur le même réseau Docker, y accède. Le mot de passe de l'utilisateur de la base doit être identique des deux côtés (`MARIADB_PASSWORD` et `WORDPRESS_DB_PASSWORD`). La tâche « Attend que MariaDB soit prête » relance, toutes les 5 secondes et au plus 12 fois, le script de vérification fourni dans l'image MariaDB, jusqu'à ce que la base réponde.

Exécutez ce playbook, puis rendez-vous sur <http://localhost> : l'assistant d'installation de WordPress vous demande de créer le compte administrateur du site.

```bash
$ ansible-playbook wordpress.yml
```

Comme nous l'avons vu, les variables sont très utiles pour paramétrer un playbook. Mais il arrive qu'elles contiennent des données sensibles (clé d'API, jeton, mot de passe). C'est pourquoi Ansible propose un mécanisme de chiffrement : vous pouvez ainsi partager vos playbooks sans vous soucier de ce problème.

**Question 1.11 :** quelles données faut-il placer dans des variables ? Lesquelles faut-il chiffrer ?

Complétez le fichier `vars.yml` créé précédemment avec les variables non sensibles (conservez `web_port` et `app_version`, toujours utilisées par `playbook-nginx.yml`), puis créez un fichier `secrets.yml` contenant les variables sensibles.

Modifiez le playbook `wordpress.yml` pour qu'il charge ces deux fichiers (`vars_files`) et utilise ces nouvelles variables.

Chiffrez le fichier `secrets.yml` avec la commande suivante, à l'aide du mot de passe de votre choix :

```bash
$ ansible-vault encrypt secrets.yml
```

**Question 1.12 :** quelle différence constatez-vous dans le fichier `secrets.yml` avant et après le chiffrement ?

Lancez désormais votre playbook avec la commande suivante, qui vous demandera ce mot de passe :

```bash
$ ansible-playbook wordpress.yml --ask-vault-pass
```

### Exercice pratique 1.1 (bonus) : Docker et la persistance

Le but de cet exercice est de revoir comment sont stockées les données écrites dans un conteneur sans volume. Les questions bonus sont numérotées B.1 à B.3, pour ne pas les confondre avec les questions 1.11 et 1.12.

**Question bonus B.1 :** expliquez le comportement de ce playbook.

```yaml
---
- name: Docker volumes
  hosts: localhost
  tasks:
    - name: Task1
      docker_container:
        name: bash
        image: bash:4.4
        command: sleep infinity
        state: started
    - name: Task2
      command: docker exec bash touch /tmp/titi
    - name: Task3
      command: docker exec bash ls /tmp
      register: result
    - name: Print stdout
      debug:
        var: result.stdout
    - name: Task4
      docker_container:
        name: bash
        state: stopped
    - name: Task5
      docker_container:
        name: bash
        state: started
    - name: Task6
      command: docker exec bash ls /tmp
      register: result2
    - name: Print stdout
      debug:
        var: result2.stdout
    - name: Task7
      docker_container:
        name: bash
        image: bash:5.2
        command: sleep infinity
        state: started
    - name: Task8
      command: docker exec bash ls /tmp
      register: result3
    - name: Print stdout
      debug:
        var: result3.stdout
```

Pour observer ce comportement, copiez ce playbook dans le fichier `docker_volumes.yml`, puis exécutez-le (sans `-K` : il n'utilise pas `become`) :

```bash
$ ansible-playbook docker_volumes.yml
```

**Question bonus B.2 :** trouvez un nom explicite pour chacune des tâches.

**Question bonus B.3 :** écrivez un script Bash qui fait la même chose que ce playbook.

### Fin de l'exercice 1 : suppression des conteneurs

Le conteneur `nginx_web_server` redémarre automatiquement (`restart_policy: always`) et occupe le port 8080, dont Terraform aura besoin dans l'exercice suivant. Supprimez les conteneurs, le réseau et les volumes créés dans cet exercice avec le playbook `nettoyage.yml` suivant :

```yaml
---
- name: Suppression des conteneurs de l'exercice 1
  hosts: localhost
  tasks:
    - name: Supprime les conteneurs
      docker_container:
        name: "{{ item }}"
        state: absent
      loop:
        - nginx_web_server
        - wordpress
        - mariadb
        - bash

    - name: Supprime le réseau
      docker_network:
        name: network_wordpress
        state: absent

    - name: Supprime les volumes
      docker_volume:
        name: "{{ item }}"
        state: absent
      loop:
        - mariadb_data
        - wordpress_data
```

```bash
$ ansible-playbook nettoyage.yml
```

Exécutez-le une seconde fois : grâce à l'idempotence, il ne signale plus aucun changement. Vérifiez enfin avec `docker ps` qu'aucun conteneur n'utilise encore le port 8080 (par exemple un conteneur d'une séance précédente).

## Exercice pratique 2 : Terraform

Terraform, développé par HashiCorp, est un outil de gestion d'infrastructure en tant que code (IaC). Il permet aux équipes informatiques et aux développeurs de définir, de déployer et de gérer l'infrastructure de leurs applications de manière automatisée, cohérente et reproductible.

Là où Ansible se concentre sur l'automatisation de la configuration et des tâches d'exploitation au moyen de playbooks, Terraform se concentre sur l'automatisation de la gestion de l'infrastructure elle-même (création, modification et suppression de VM, d'espaces de stockage…).

Dans Terraform, un *provider* est un plugin qui permet de gérer les ressources d'un service cloud ou d'une API. Une ressource est un élément configurable, comme une instance de machine virtuelle, qu'un provider donné peut créer, mettre à jour ou supprimer conformément à la configuration Terraform.

Installez la clé GPG de HashiCorp :

```bash
$ wget -O- https://apt.releases.hashicorp.com/gpg | \
    sudo gpg --yes --dearmor \
    -o /usr/share/keyrings/hashicorp-archive-keyring.gpg
```

Ajoutez le dépôt officiel :

```bash
$ echo "deb [signed-by=/usr/share/keyrings/hashicorp-archive-keyring.gpg] \
https://apt.releases.hashicorp.com $(lsb_release -cs) main" | \
    sudo tee /etc/apt/sources.list.d/hashicorp.list
```

Mettez à jour la liste des paquets :

```bash
$ sudo apt-get update
```

Installez Terraform :

```bash
$ sudo apt-get install terraform
```

Vérifiez l'installation :

```bash
$ terraform --version
```

Dans ce TP, nous allons utiliser le provider Docker en local. Mais la plupart du temps, Terraform sert à configurer des ressources dans le cloud : on utilise alors des providers comme AWS, GCP ou Azure.

Créez le répertoire de travail de Terraform et placez-vous dedans :

```bash
$ mkdir -p ~/R5A09/"${MonNom:?}"/terraform && cd $_
```

Créez-y le fichier `~/R5A09/${MonNom}/terraform/main.tf` :

```hcl
terraform {
  required_providers {
    docker = {
      source  = "kreuzwerker/docker"
      version = "~> 3.0.1"
    }
  }
}

provider "docker" {}

resource "docker_image" "nginx" {
  name         = "nginx:latest"
  keep_locally = false
}

resource "docker_container" "nginx" {
  image = docker_image.nginx.image_id
  name  = "Wazabi"
  ports {
    internal = 80
    external = 8000
  }
}
```

Le fichier `main.tf` contient toute la configuration dont Terraform a besoin pour déployer un serveur web via le provider Docker.

Le bloc `terraform {}` contient les paramètres généraux de Terraform, comme les providers nécessaires pour provisionner l'infrastructure. Les blocs `provider` configurent les plugins qui gèrent les ressources, tandis que les blocs `resource` définissent les composants de l'infrastructure, comme des conteneurs Docker ou des VM.

Pour une nouvelle configuration, commencez par initialiser le répertoire avec la commande `terraform init` :

```bash
$ terraform init
```

Cette commande télécharge et installe les providers dans votre répertoire courant.

**Question 2.1 :** que contiennent le répertoire `.terraform` et le fichier `.terraform.lock.hcl` ?

Il est recommandé d'utiliser la commande `terraform fmt`, qui vérifie et corrige la mise en forme de votre configuration selon les conventions de Terraform (la commande `terraform validate` en vérifie, elle, la validité). Ici, le fichier `main.tf` est correct.

**Question 2.2 :** quel est le résultat de la commande `terraform fmt` ?

Vous pouvez maintenant appliquer votre configuration à l'aide de la commande :

```bash
$ terraform apply
```

Avant d'appliquer les modifications, Terraform affiche le plan d'exécution, qui décrit les actions qu'il va effectuer. Le format de sortie ressemble au format *diff* produit par des outils comme Git. Terraform compare l'état actuel de votre machine à l'état attendu, décrit dans votre configuration. Dans la sortie, le `+` à côté de `docker_container.nginx` signifie que Terraform va créer cette ressource. En dessous figurent les attributs qui seront définis. Tous ne peuvent pas être connus à l'avance, comme l'identifiant du conteneur, attribué par Docker à sa création.

**Question 2.2 bonus :** pourquoi la commande affiche-t-elle `(known after apply)` ?

Terraform attend votre validation avant d'exécuter son plan. En production, il est essentiel de bien relire le plan d'exécution : une erreur de configuration pourrait causer de gros dommages à votre infrastructure !

Ici, vous pouvez accepter le plan en tapant `yes`.

Vous venez de déployer votre première infrastructure as code avec Terraform. Vous pouvez tester le site web dans votre navigateur : <http://localhost:8000>.

Lorsque vous avez appliqué votre configuration, Terraform a écrit des données dans un fichier nommé `terraform.tfstate`. Terraform y stocke les identifiants et les propriétés des ressources qu'il gère, afin de pouvoir les mettre à jour ou les détruire par la suite. Ce fichier d'état est le seul moyen pour Terraform de suivre les ressources qu'il gère, et il contient souvent des informations sensibles : vous devez donc le stocker de manière sûre et en réserver l'accès aux seuls membres de confiance de l'équipe chargés de gérer votre infrastructure.

Vous pouvez consulter cet état avec la commande :

```bash
$ terraform show
```

**Question 2.3 :** quel est le nom du conteneur qui a été créé ?

```bash
$ docker ps
```

Nous allons maintenant utiliser Terraform pour modifier notre infrastructure.

Modifiez le fichier `~/R5A09/${MonNom}/terraform/main.tf` pour que le conteneur expose le port HTTP sur le port 8080 (libéré à la fin de l'exercice 1), et renommez le conteneur en `Nginx_By_Terraform`.

Déployez ensuite votre infrastructure avec la commande :

```bash
$ terraform apply
```

**Question 2.4 :** donnez le résultat de cette commande et testez l'application.

Pour terminer cette séance, supprimez votre infrastructure avec la commande :

```bash
$ terraform destroy
```

## Nettoyage

Avant la fin de la séance, sauvegardez tous les fichiers dont vous aurez besoin pour rédiger votre compte rendu (dont `seance2.txt`), puis supprimez toutes vos traces. Ne supprimez que ce que vous avez créé : le poste est partagé.

Si ce n'est pas déjà fait, détruisez l'infrastructure Terraform. Faites-le avant de supprimer vos fichiers : Terraform a besoin de `terraform.tfstate` pour retrouver ses ressources.

```bash
$ cd ~/R5A09/${MonNom}/terraform && terraform destroy
```

Si ce n'est pas déjà fait, supprimez les conteneurs, le réseau et les volumes de l'exercice 1 :

```bash
$ cd ~/R5A09/${MonNom}/ansible && ansible-playbook nettoyage.yml
```

Supprimez les images téléchargées pendant ce TD (Docker signale celles qui sont déjà supprimées ou encore utilisées : ignorez ces messages) :

```bash
$ docker image rm nginx:latest mariadb:11.4 wordpress:6 bash:4.4 bash:5.2
```

Supprimez les répertoires créés par les playbooks :

```bash
$ sudo rm -r /opt/nginx_html
$ rm -r /tmp/my_dir
```

Vérifiez que les comptes `user1` à `user4` et leurs répertoires personnels ont bien été supprimés (question 1.10) : la première commande ne doit rien afficher, la seconde doit répondre qu'aucun fichier ne correspond.

```bash
$ getent passwd user1 user2 user3 user4
$ ls -d /home/user[1-4]
```

Sinon, supprimez chaque compte restant avec `sudo userdel -r user1` (de même pour les autres) et chaque répertoire restant avec `sudo rm -r /home/user1`.

Si vous vous êtes connecté à Docker Hub, déconnectez-vous, pour ne pas laisser votre jeton d'accès sur le poste :

```bash
$ docker logout
```

Retirez du fichier `~/.ssh/authorized_keys` les clés ajoutées pendant la séance (la vôtre et, le cas échéant, celle de votre voisin), en supprimant les lignes correspondantes :

```bash
$ vi ~/.ssh/authorized_keys
```

Quittez la commande `script`, puis supprimez vos fichiers :

```bash
$ exit
$ cd ~ && rm -fr ~/R5A09/"${MonNom:?}"
```

Supprimez votre historique :

```bash
$ history -d ${votre_premiere_ligne}-${votre_derniere_ligne}
```

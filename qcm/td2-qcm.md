# TD 2 : QCM

*Exercices théoriques du [TD 2 : Infrastructure as Code](../td/td2-infrastructure-as-code.md).*

## Mode d'emploi

- Pour répondre, téléchargez ce fichier (bouton **Download raw file** de GitHub) ou copiez son contenu dans votre compte rendu.
- Cochez une case en remplaçant `[ ]` par `[x]`. Remplissez les cellules vides des tableaux.
- Sauf mention « plusieurs réponses possibles », une seule réponse est correcte.
- Pour vous aider : chapitres [6](../cours/06-cloud.md), [7](../cours/07-le-devops.md) et [8](../cours/08-infrastructure-as-code.md) du cours, ainsi que les liens donnés au début du TD.

## Exercice 1 : cloud et IaC

### 1.1 Quelle définition correspond au *cloud computing* ?

- [ ] Un serveur de fichiers partagé, installé dans les locaux de l'entreprise
- [ ] Un réseau privé virtuel chiffré entre deux sites distants
- [ ] L'accès à la demande, via Internet, à des ressources informatiques (serveurs, stockage, réseau, applications…) hébergées par un fournisseur et facturées par abonnement ou à l'usage
- [ ] Un logiciel installé sur chaque poste et synchronisé chaque nuit

### 1.2 Pour chaque modèle de service, qui gère le système d'exploitation, et qui gère l'application ?

Répondez par **client** ou **fournisseur**.

| Modèle | Système d'exploitation | Application |
| --- | --- | --- |
| IaaS (*Infrastructure as a Service*) | | |
| PaaS (*Platform as a Service*) | | |
| SaaS (*Software as a Service*) | | |

### 1.3 Classez chaque offre d'OVHcloud : **IaaS**, **PaaS** ou **SaaS**

| Offre | IaaS, PaaS ou SaaS ? |
| --- | --- |
| Instances Public Cloud | |
| E-mail Exchange | |
| Public Cloud Databases (PostgreSQL, MongoDB, Kafka managés) | |
| VPS | |
| Zimbra | |
| Serveur dédié | |
| Hosted Private Cloud VMware | |

### 1.4 L'offre Managed Kubernetes, où le fournisseur exploite l'orchestrateur et le client gère ses conteneurs, relève plutôt :

- [ ] de l'IaaS
- [ ] du SaaS
- [ ] du CaaS (*Containers as a Service*)
- [ ] du DaaS (*Desktop as a Service*)

### 1.5 Qu'est-ce qu'une infrastructure multi-provider ?

- [ ] Une infrastructure répartie sur plusieurs régions d'un même fournisseur
- [ ] Un environnement où une organisation utilise des ressources provenant de plusieurs fournisseurs de cloud ou d'infrastructure
- [ ] Un fournisseur unique qui revend les services de plusieurs autres
- [ ] Plusieurs comptes ouverts chez un même fournisseur

### 1.6 Infrastructure multi-provider : avantage (A) ou inconvénient (I) ?

| Affirmation | A ou I |
| --- | --- |
| Optimisation des coûts | |
| Gestion technique plus complexe | |
| Pas de dépendance à un seul fournisseur | |
| Meilleure adéquation des services aux besoins | |
| Gestion administrative et facturations multipliées | |
| Conformité réglementaire | |
| Performances des liens réseau entre les fournisseurs | |

### 1.7 Associez chaque modèle de déploiement à sa définition

| N° | Modèle |
| --- | --- |
| 1 | Cloud privé |
| 2 | Cloud public |
| 3 | Cloud hybride |

| Lettre | Définition |
| --- | --- |
| A | Combinaison de cloud privé et de cloud public, qui permet de déplacer des charges de travail de l'un à l'autre selon les besoins. |
| B | Environnement dédié à une seule organisation, géré en interne ou par un tiers, avec un niveau élevé de contrôle et de personnalisation. |
| C | Infrastructure mutualisée entre de nombreux clients, accessible via Internet, gérée par un fournisseur et payée à la consommation. |

**Vos réponses :**

| 1 | 2 | 3 |
| --- | --- | --- |
|   |   |   |

## Exercice 2 : Ansible et Terraform

### 2.1 Quelles affirmations sur Ansible sont vraies ? *(plusieurs réponses possibles)*

- [ ] Ansible est sans agent : rien n'est à installer sur les serveurs gérés.
- [ ] Ansible se connecte aux serveurs distants via SSH.
- [ ] Les *playbooks* Ansible s'écrivent en HCL.
- [ ] Les *playbooks* Ansible s'écrivent en YAML.
- [ ] Ansible sert surtout à la gestion de configuration et au déploiement d'applications.

### 2.2 Quelles affirmations sur Terraform sont vraies ? *(plusieurs réponses possibles)*

- [ ] Terraform décrit l'infrastructure dans des fichiers déclaratifs, en langage HCL.
- [ ] Terraform ne fonctionne qu'avec AWS.
- [ ] La commande `terraform apply` crée l'infrastructure décrite.
- [ ] Terraform peut provisionner une infrastructure dans le cloud ou sur site.
- [ ] Terraform est sous licence BSL depuis 2023 ; son *fork* OpenTofu est resté open source.

### 2.3 Dans un usage conjoint d'Ansible et de Terraform, quelle répartition des rôles est la plus courante ?

- [ ] Ansible crée les machines virtuelles, puis Terraform installe les logiciels dessus.
- [ ] Terraform provisionne l'infrastructure (VM, réseaux…), puis Ansible configure ces ressources.
- [ ] Les deux outils font la même chose : on n'en utilise jamais deux à la fois.
- [ ] Terraform supervise les serveurs configurés par Ansible.

### 2.4 Approche impérative (I) ou déclarative (D) ?

| Exemple | I ou D |
| --- | --- |
| Un fichier de configuration Terraform | |
| Un script Bash | |
| Un manifeste Kubernetes | |
| Une suite de commandes `docker run` | |
| Un fichier `docker-compose.yml` | |
| La commande `aws ec2 run-instances` | |

### 2.5 Comment situer Ansible entre ces deux approches ?

- [ ] Purement impératif : chaque tâche est une commande shell.
- [ ] Purement déclaratif : l'ordre des tâches n'a aucune importance.
- [ ] Entre les deux : ses modules sont déclaratifs (on décrit l'état voulu), mais les tâches d'un *playbook* s'exécutent dans l'ordre écrit.
- [ ] Ni l'un ni l'autre : Ansible n'est pas un outil d'Infrastructure as Code.

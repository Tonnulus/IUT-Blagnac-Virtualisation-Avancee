# TD 1 : QCM

*Exercices théoriques du [TD 1 : Introduction à la virtualisation avancée](../td/td1-introduction-a-la-virtualisation-avancee.md).*

## Mode d'emploi

- Pour répondre, téléchargez ce fichier (bouton **Download raw file** de GitHub) ou copiez son contenu dans votre compte rendu.
- Cochez une case en remplaçant `[ ]` par `[x]`. Remplissez les cellules vides des tableaux.
- Sauf mention « plusieurs réponses possibles », une seule réponse est correcte.
- Pour vous aider : chapitres [1](../cours/01-virtualisation-concepts-generaux.md), [2](../cours/02-serveur-virtualise.md), [3](../cours/03-stockage-virtualise.md) et [4](../cours/04-conteneurisation.md) du cours, ainsi que les liens donnés au début du TD.

## Exercice 1 : définitions

### 1.1 Associez chaque terme à sa définition

| N° | Terme |
| --- | --- |
| 1 | DevOps |
| 2 | Externalisation |
| 3 | Cloud |
| 4 | Partitionnement (au sens de la virtualisation) |
| 5 | Virtualisation |
| 6 | Émulation |
| 7 | Conteneurisation |
| 8 | Hôte (*host*) |
| 9 | Invité (*guest*) |
| 10 | Hyperviseur |

| Lettre | Définition |
| --- | --- |
| A | Machine virtuelle et son système d'exploitation : environnement isolé qui utilise les ressources virtuelles mises à sa disposition. |
| B | Fourniture de ressources informatiques (calcul, stockage, réseau, logiciels) accessibles par le réseau, à la demande, de manière élastique et souvent facturées à l'usage. |
| C | Reproduction par logiciel du comportement d'un autre système, souvent d'une autre architecture matérielle : chaque instruction est traduite. |
| D | Culture et ensemble de pratiques qui rapprochent les équipes de développement et d'exploitation pour automatiser et accélérer la livraison des logiciels. |
| E | Logiciel qui héberge une ou plusieurs machines virtuelles en partageant entre elles les ressources matérielles, tout en les isolant les unes des autres. |
| F | Division d'une ressource physique en plusieurs ressources virtuelles autonomes, par exemple les partitions logiques (LPAR) d'un serveur. |
| G | Machine physique, avec son hyperviseur ou son système d'exploitation, qui fournit les ressources matérielles. |
| H | Fait de confier certaines activités à des prestataires spécialisés, pour réduire les coûts ou accéder à des compétences particulières. |
| I | Exécution d'applications isolées qui partagent le noyau de l'hôte, sans système d'exploitation complet pour chacune. |
| J | Création d'une couche d'abstraction au-dessus du matériel, qui permet de diviser ses composants (processeur, mémoire, stockage, réseau) en composants virtuels. |

**Vos réponses :**

| 1 | 2 | 3 | 4 | 5 | 6 | 7 | 8 | 9 | 10 |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
|   |   |   |   |   |   |   |   |   |   |

### 1.2 Associez chaque terme à sa définition

| N° | Terme |
| --- | --- |
| 11 | Hyperviseur de type 1 |
| 12 | Hyperviseur de type 2 |
| 13 | Paravirtualisation |
| 14 | Pass-through |
| 15 | Nœud |
| 16 | Cluster |
| 17 | Grille |
| 18 | Clonage de VM |
| 19 | Snapshot |
| 20 | Green IT |

| Lettre | Définition |
| --- | --- |
| K | Système distribué à grande échelle, qui fédère des ressources hétérogènes, dispersées géographiquement et appartenant souvent à plusieurs organisations. |
| L | Attribution directe et exclusive d'un périphérique matériel (carte graphique, carte réseau…) à une machine virtuelle, grâce à l'IOMMU. |
| M | Utilisation responsable des technologies de l'information pour réduire leur impact environnemental. |
| N | Hyperviseur qui s'exécute comme une application sur un système d'exploitation hôte (VirtualBox, VMware Workstation). |
| O | Capture de l'état d'une machine virtuelle à un instant donné, pour pouvoir y revenir plus tard. |
| P | Dispositif capable de recevoir, de stocker, de traiter ou de transmettre des données ; chacune des machines d'un cluster. |
| Q | Hyperviseur qui s'exécute directement sur le matériel, sans système d'exploitation hôte (VMware ESXi, Hyper-V, KVM, Xen). |
| R | Création d'une copie identique d'une machine virtuelle existante. |
| S | Le système invité sait qu'il est virtualisé et coopère avec l'hyperviseur au travers d'une interface dédiée (*hypercalls*, pilotes virtio). |
| T | Ensemble de nœuds interconnectés, généralement proches et homogènes, qui travaillent ensemble et sont vus comme un seul système. |

**Vos réponses :**

| 11 | 12 | 13 | 14 | 15 | 16 | 17 | 18 | 19 | 20 |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
|   |   |   |   |   |   |   |   |   |   |

### 1.3 Vrai ou faux ?

| Affirmation | Vrai | Faux |
| --- | --- | --- |
| L'émulation est nettement plus lente que la virtualisation. | [ ] | [ ] |
| Un snapshot est une sauvegarde : il reste utilisable si le disque d'origine est perdu. | [ ] | [ ] |
| Un périphérique attribué en pass-through à une VM peut être partagé avec les autres VM. | [ ] | [ ] |
| La paravirtualisation améliore les performances, car l'invité n'utilise plus de matériel émulé. | [ ] | [ ] |

## Exercice 2 : avantages et inconvénients

Pour chaque affirmation, indiquez s'il s'agit d'un **avantage (A)** ou d'un **inconvénient (I)** de la solution.

### 2.1 Déployer une application sans virtualisation

| Affirmation | A ou I |
| --- | --- |
| Performance | |
| Accès direct au matériel | |
| Ressources souvent sous-utilisées | |
| Coût : un serveur physique par application | |
| Moins de complexité | |
| Difficultés de maintenance | |
| Aucune isolation si plusieurs applications partagent le même serveur | |
| Aucune licence supplémentaire nécessaire | |
| Séparation physique | |
| Difficulté à produire un environnement de test ou de préproduction | |

### 2.2 Déployer une application sur un serveur virtualisé

| Affirmation | A ou I |
| --- | --- |
| Mutualisation des ressources physiques | |
| Isolation des environnements | |
| Surcoût (*overhead*) : performances dégradées | |
| Migration à chaud, snapshots, sauvegarde | |
| Dépendance vis-à-vis de l'hyperviseur | |
| Complexité accrue | |
| Surallocation (*over-provisioning*) des ressources | |
| Chaque machine virtuelle embarque un système d'exploitation complet | |
| Gestion plus fine des ressources | |
| Tests et développement simplifiés | |
| Licences | |

### 2.3 Déployer une application via des conteneurs

| Affirmation | A ou I |
| --- | --- |
| Isolation légère | |
| Portabilité | |
| Isolation plus faible qu'avec une VM (noyau partagé) | |
| Complexité du réseau | |
| Rapidité de déploiement | |
| Un conteneur Linux a besoin d'un noyau Linux | |
| Gestion simplifiée | |
| Utilisation efficace des ressources | |

### 2.4 Quel est l'ordre de grandeur du surcoût (*overhead*) d'un hyperviseur de type 1 avec l'assistance matérielle à la virtualisation ?

- [ ] Moins de 0,1 %
- [ ] 1 à 5 %
- [ ] 20 à 30 %
- [ ] Environ 50 %

## Exercice 3 : les différents types de virtualisation

### 3.1 Classez chaque solution selon ce qu'elle virtualise : **serveur**, **bureau** ou **stockage**

| Solution | Serveur, bureau ou stockage ? |
| --- | --- |
| VMware ESXi (vSphere) | |
| Citrix Virtual Apps and Desktops | |
| Ceph | |
| Microsoft Hyper-V | |
| VMware Horizon | |
| LVM | |
| Proxmox VE (KVM) | |
| MinIO (stockage objet compatible S3) | |
| Microsoft Remote Desktop Services (RDS) | |
| VMware vSAN | |

### 3.2 Quels sont les intérêts de la virtualisation du poste de travail (VDI) ? *(plusieurs réponses possibles)*

- [ ] Administration et mises à jour centralisées
- [ ] Les données restent dans le centre de données
- [ ] Accès au poste depuis n'importe quel terminal
- [ ] Le poste fonctionne sans aucune connexion réseau
- [ ] Les performances graphiques dépassent toujours celles d'un poste local

### 3.3 Quels sont les intérêts de la virtualisation du stockage ? *(plusieurs réponses possibles)*

- [ ] Agréger des disques physiques en pools de stockage virtuels
- [ ] Simplifier l'évolutivité
- [ ] Améliorer la redondance et la gestion des données
- [ ] Rendre les sauvegardes inutiles

### 3.4 Associez chaque réseau virtuel à sa définition

| N° | Réseau |
| --- | --- |
| 1 | VLAN (*Virtual Local Area Network*) |
| 2 | VPN (*Virtual Private Network*) |
| 3 | SDN (*Software-Defined Networking*) |

| Lettre | Définition |
| --- | --- |
| A | Connexion sécurisée et chiffrée entre des emplacements distants via Internet, qui donne accès aux ressources internes. |
| B | Modèle où le contrôle du réseau est séparé du matériel qui achemine les paquets, pour une gestion centralisée et programmable. |
| C | Segmentation logique d'un réseau physique en plusieurs sous-réseaux virtuels isolés les uns des autres. |

**Vos réponses :**

| 1 | 2 | 3 |
| --- | --- | --- |
|   |   |   |

### 3.5 Le SDN sépare :

- [ ] le réseau public et le réseau privé
- [ ] le plan de contrôle et le plan de données
- [ ] la couche 2 et la couche 3 du modèle OSI
- [ ] le trafic entrant et le trafic sortant

## Exercice 4 : la conteneurisation avec Docker

### 4.1 Associez chaque mécanisme à son rôle dans la conteneurisation

| N° | Mécanisme |
| --- | --- |
| 1 | Cgroups (*control groups*) |
| 2 | Namespaces |
| 3 | chroot |
| 4 | UnionFS (système de fichiers en couches) |
| 5 | Netfilter |
| 6 | systemd |

| Lettre | Rôle |
| --- | --- |
| A | Superpose plusieurs systèmes de fichiers : des couches d'image en lecture seule, partagées, et une couche en écriture propre à chaque conteneur (*copy-on-write*). |
| B | Système d'initialisation qui gère, sur l'hôte, le démon du moteur de conteneurs (`docker.service`) et la hiérarchie des cgroups. |
| C | Limite et comptabilise les ressources (processeur, mémoire, entrées-sorties, nombre de processus) allouées à un groupe de processus. |
| D | Framework de filtrage de paquets du noyau : via iptables ou nftables, il fait la traduction d'adresses (NAT) et publie les ports des conteneurs sur l'hôte. |
| E | Change le répertoire racine d'un processus et de ses fils. |
| F | Donne à chaque conteneur sa propre vue du système : processus, interfaces réseau, points de montage, nom d'hôte, utilisateurs, IPC. |

**Vos réponses :**

| 1 | 2 | 3 | 4 | 5 | 6 |
| --- | --- | --- | --- | --- | --- |
|   |   |   |   |   |   |

### 4.2 Quel mécanisme empêche un conteneur de monopoliser toute la mémoire de l'hôte ?

- [ ] Les namespaces
- [ ] Les cgroups
- [ ] chroot
- [ ] Netfilter

### 4.3 Grâce à quel mécanisme le processus principal d'un conteneur voit-il son propre PID 1 ?

- [ ] Les cgroups
- [ ] UnionFS
- [ ] Les namespaces
- [ ] systemd

### 4.4 Quel pilote de stockage Docker utilise-t-il aujourd'hui pour ses couches ?

- [ ] `aufs`
- [ ] `overlay2` (OverlayFS)
- [ ] `devicemapper`
- [ ] `ntfs`

### 4.5 Vrai ou faux ?

| Affirmation | Vrai | Faux |
| --- | --- | --- |
| Les conteneurs d'un même hôte exécutent chacun leur propre noyau. | [ ] | [ ] |
| chroot seul suffit à isoler un conteneur de façon sûre. | [ ] | [ ] |
| La commande `docker run -p 8080:80` s'appuie sur Netfilter pour publier le port. | [ ] | [ ] |
| On évite en général d'exécuter systemd à l'intérieur d'un conteneur. | [ ] | [ ] |
| Les couches d'une image sont partagées entre tous les conteneurs qui l'utilisent. | [ ] | [ ] |

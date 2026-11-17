*[Virtualisation avancée, les notions essentielles](README.md) › Chapitre 1*

# 1. Virtualisation, concepts généraux

## Historique

La virtualisation existe presque depuis les débuts de l'informatique. Son principe est simple : intercaler une couche d'abstraction entre le matériel et les logiciels, afin de faire fonctionner plusieurs systèmes d'exploitation ou applications sur une même machine.

Tout commence dans les années 1960 chez IBM, avec le CP-40, capable de faire tourner plusieurs systèmes d'exploitation indépendants sur un même ordinateur central (*mainframe*). La virtualisation moderne prend son essor à la fin des années 1990 sur l'architecture x86, celle des PC et des serveurs du marché : VMware parvient alors à exécuter plusieurs systèmes d'exploitation sur une seule machine physique. La technologie s'est ensuite diversifiée (virtualisation au niveau du système d'exploitation, virtualisation d'applications, conteneurs). Elle a profondément changé l'exploitation des infrastructures : meilleure utilisation du matériel, gestion plus souple, déploiements plus rapides.

**Les dates clés :**

| Année | Événement |
| --- | --- |
| 1967 | IBM met en service le CP-40, l'un des premiers systèmes de virtualisation, qui exécute plusieurs instances de système d'exploitation sur un mainframe. |
| 1972 | IBM commercialise VM/370, qui popularise les machines virtuelles sur ses ordinateurs centraux. |
| 1999 | VMware (fondée en 1998 par Diane Greene, Mendel Rosenblum, Scott Devine, Edouard Bugnion et Ellen Wang) lance VMware Workstation : la virtualisation arrive sur les PC x86. |
| 2003 | Première version publique de Xen, hyperviseur open source issu de l'université de Cambridge, qui popularise la paravirtualisation sur les serveurs. |
| 2006 | Amazon Web Services lance EC2 (*Elastic Compute Cloud*) : la location de machines virtuelles à la demande donne naissance au cloud computing moderne. |
| 2007 | KVM est intégré au noyau Linux, qui devient lui-même un hyperviseur. |
| 2009 | VMware Infrastructure devient VMware vSphere, plateforme de référence pour la virtualisation des centres de données. |
| 2013 | Lancement de Docker, qui démocratise les conteneurs et accompagne l'essor du DevOps. |
| 2014 | Google dévoile Kubernetes, orchestrateur de conteneurs open source, confié en 2015 à la Cloud Native Computing Foundation (CNCF). |
| 2016 | Microsoft lance le Windows Subsystem for Linux (WSL). Annoncé en 2019 et disponible en 2020, WSL 2 exécute un vrai noyau Linux dans une machine virtuelle légère. |
| 2020 | Apple annonce le passage des Mac à ses processeurs ARM (Apple Silicon) : les outils de virtualisation et d'émulation doivent s'adapter à cette nouvelle architecture. |

## Virtualisation et émulation

La virtualisation consiste à créer une version virtuelle d'une ressource (système d'exploitation, serveur, réseau, stockage) au moyen d'une couche d'abstraction. Il ne faut pas la confondre avec l'émulation, même si les deux sont souvent combinées :

- **Émulation** : simulation logicielle d'un matériel ou d'un environnement différent de celui de l'hôte. Elle permet d'exécuter un programme sur un matériel normalement incompatible (par exemple un logiciel ARM sur un processeur x86). Elle est souple mais coûteuse en performances.
- **Virtualisation** : création d'instances isolées qui s'exécutent sur le même type de matériel que l'hôte. Les instructions des invités s'exécutent directement sur le processeur, ce qui limite la perte de performances.

Deux termes reviendront tout au long du cours :

- **Hôte** (*host*) : le système physique sur lequel s'exécutent les machines virtuelles ou les conteneurs.
- **Invité** (*guest*) : le système d'exploitation ou l'application qui s'exécute à l'intérieur d'une machine virtuelle ou d'un conteneur.

La plupart des solutions de virtualisation savent aussi émuler certains périphériques (carte réseau, contrôleur disque, carte graphique) pour les présenter aux invités.

## Avantages et inconvénients

### Les avantages

- **Optimisation des ressources** : plusieurs machines virtuelles (VM) partagent une même machine physique. Le matériel, rarement utilisé à pleine charge par un seul système, est mieux rentabilisé.
- **Isolation et sécurité** : chaque VM est isolée des autres. Une panne ou une compromission sur l'une n'affecte pas directement ses voisines, ce qui limite la propagation des logiciels malveillants.
- **Flexibilité et agilité** : créer, dupliquer, déplacer ou supprimer une VM prend quelques minutes. Les déploiements et les tests en environnement isolé s'en trouvent accélérés.
- **Économies** : moins de serveurs physiques, c'est moins d'achats, de maintenance, de place en salle et de consommation électrique.
- **Migration et haute disponibilité** : une VM peut être déplacée d'un hôte à un autre, y compris à chaud. On équilibre ainsi la charge et on redémarre les VM ailleurs en cas de panne matérielle.
- **Indépendance vis-à-vis du matériel** : l'invité voit un matériel virtuel standard, ce qui facilite le remplacement des serveurs physiques.

### Les inconvénients

- **Surcoût de performances** (*overhead*) : la couche d'abstraction entre le matériel et l'application consomme un peu de ressources.
- **Complexité** : l'exploitation d'une infrastructure virtualisée demande des compétences spécifiques.
- **Dépendance à l'hyperviseur** : si l'hyperviseur ou son hôte tombe, toutes les VM hébergées sont touchées. On parle de point unique de défaillance (*single point of failure*).
- **Licences logicielles** : certains éditeurs facturent selon le nombre de cœurs physiques de l'hôte ou de VM. Rester en conformité peut devenir complexe et coûteux.
- **Sécurité** : l'isolation renforce la sécurité, mais une mauvaise configuration de l'hyperviseur ou des réseaux virtuels ouvre de nouveaux risques.
- **Migrations délicates** : certaines applications demandent des ajustements importants pour passer d'un serveur physique à un environnement virtuel sans perdre en performances.

Bien maîtrisée, la virtualisation apporte efficacité, flexibilité et indépendance vis-à-vis du matériel. Ses limites doivent toutefois être connues pour faire les bons choix d'architecture.

## Les domaines d'application

La virtualisation s'applique à presque toutes les couches d'un système d'information.

### Virtualisation des serveurs

C'est l'usage le plus courant. Des solutions comme VMware vSphere, Microsoft Hyper-V ou Proxmox Virtual Environment découpent un serveur physique en plusieurs machines virtuelles indépendantes. Chez un hébergeur, une VM louée est souvent appelée VPS (*Virtual Private Server*).

### Virtualisation des réseaux

Elle crée des réseaux virtuels isolés au-dessus d'une infrastructure physique commune. On segmente et sécurise ainsi les flux, et on simplifie la configuration. Commutateurs, routeurs et pare-feu peuvent tous être virtualisés. Trois technologies sont à connaître :

- **VLAN** (*Virtual Local Area Network*, norme IEEE 802.1Q) : découpage logique d'un réseau physique en plusieurs réseaux distincts, au niveau 2 du modèle OSI. Chaque VLAN est isolé des autres, ce qui améliore la sécurité et la maîtrise du trafic.
- **VPN** (*Virtual Private Network*) : connexion chiffrée entre des sites ou des utilisateurs distants, au travers d'Internet, pour accéder aux ressources internes comme si l'on était sur place. Il s'agit le plus souvent d'une virtualisation de la couche 3 du modèle OSI, avec des protocoles comme IPsec, WireGuard ou OpenVPN (TLS). Les opérateurs proposent aussi des VPN MPLS entre sites.
- **SDN** (*Software-Defined Networking*) : le plan de contrôle (les décisions de routage) est séparé du plan de données (l'acheminement des paquets) et centralisé dans un contrôleur logiciel. Le réseau devient programmable, et un même équipement peut changer de rôle selon sa configuration.

### Virtualisation du stockage

Les disques physiques sont regroupés en un pool de stockage virtuel. On alloue ensuite de l'espace à la demande, sans être limité par la capacité d'un disque en particulier. Ceph, VMware vSAN ou, à l'échelle d'un serveur, LVM permettent cet usage (voir le [chapitre 3](03-stockage-virtualise.md)).

### Virtualisation d'applications

Les applications s'exécutent dans des environnements isolés, indépendamment du système sous-jacent, ce qui simplifie leur déploiement et leur compatibilité. Les conteneurs (Docker, LXC) en sont aujourd'hui la forme la plus répandue. Il existe aussi des outils de virtualisation applicative au sens strict, comme Microsoft App-V, qui encapsulent une application de bureau.

### Virtualisation du poste de travail (VDI)

La VDI (*Virtual Desktop Infrastructure*) héberge les postes de travail dans le centre de données. Les utilisateurs accèdent à leur bureau depuis n'importe quel terminal, les données restent centralisées et l'administration est simplifiée. Citrix Virtual Apps and Desktops et Microsoft Remote Desktop Services sont des solutions répandues.

> **À retenir**
>
> - La virtualisation intercale une couche d'abstraction entre le matériel et les logiciels pour partager une même machine entre plusieurs systèmes isolés.
> - L'émulation simule un matériel *différent* ; la virtualisation exécute des invités sur le *même* type de matériel, avec peu de pertes.
> - Elle s'applique aux serveurs, aux réseaux (VLAN, VPN, SDN), au stockage, aux applications et aux postes de travail.

**Pour aller plus loin**

- [Red Hat, « Qu'est-ce que la virtualisation ? »](https://www.redhat.com/fr/topics/virtualization)
- [IBM, « Qu'est-ce que la virtualisation ? »](https://www.ibm.com/fr-fr/topics/virtualization)

---

[← Introduction](README.md) · [Sommaire](README.md#sommaire) · [2. Serveur virtualisé →](02-serveur-virtualise.md)

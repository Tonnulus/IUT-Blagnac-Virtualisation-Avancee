*[Virtualisation avancée, les notions essentielles](README.md) › Chapitre 10*

# 10. Glossaire

La dernière colonne renvoie au chapitre où la notion est expliquée.

| Terme | Définition | Chapitre |
| --- | --- | --- |
| **Budget d'erreur** | Écart entre 100 % et le SLO : la quantité d'indisponibilité que l'on peut « dépenser » avant de devoir privilégier la stabilité. | [9](09-monitoring.md) |
| **Cgroups** | Mécanisme du noyau Linux qui limite et comptabilise les ressources (CPU, mémoire, E/S) d'un groupe de processus. | [4](04-conteneurisation.md) |
| **CI/CD** | Intégration continue / livraison (ou déploiement) continue : automatisation des étapes qui mènent d'une modification du code à sa mise en production (build, tests, déploiement). | [7](07-le-devops.md), [8](08-infrastructure-as-code.md) |
| **Cloud hybride** | Combinaison d'un cloud privé et d'un cloud public. On parle de multicloud lorsqu'on combine plusieurs fournisseurs de cloud public. | [6](06-cloud.md) |
| **Cloud privé** | Infrastructure cloud dédiée à une seule organisation, sur site ou dans un centre de données privé. | [6](06-cloud.md) |
| **Cloud public** | Infrastructure cloud mise à disposition de tous les clients d'un fournisseur, via Internet, et partagée entre eux. | [6](06-cloud.md) |
| **Conteneur** | Instance en cours d'exécution d'une image, isolée par le noyau de l'hôte qu'elle partage. | [4](04-conteneurisation.md) |
| **Deployment** | Ressource Kubernetes qui maintient un nombre de pods et gère leurs mises à jour progressives. | [5](05-orchestration.md) |
| **DevOps** | Culture et ensemble de pratiques qui rapprochent le développement et l'exploitation pour livrer plus souvent, plus vite et avec une meilleure qualité. | [7](07-le-devops.md) |
| **Docker Compose** | Outil qui décrit dans un fichier YAML une application multi-conteneurs (services, réseaux, volumes) et la démarre en une commande. | [4](04-conteneurisation.md) |
| **Dockerfile** | Fichier texte qui décrit, instruction par instruction, comment construire une image. | [4](04-conteneurisation.md) |
| **Émulation** | Simulation logicielle d'un matériel différent de celui de l'hôte. | [1](01-virtualisation-concepts-generaux.md) |
| **Extent** | Bloc de taille fixe (4 Mio par défaut), unité d'allocation de LVM. | [3](03-stockage-virtualise.md) |
| **FinOps** | Pilotage financier du cloud, pour éviter que des ressources oubliées ou surdimensionnées ne fassent grimper la facture. | [6](06-cloud.md) |
| **Helm** | Gestionnaire de paquets de Kubernetes ; ses *charts* définissent, installent et mettent à jour des applications complètes. | [5](05-orchestration.md) |
| **Hôte** | Système qui exécute les machines virtuelles ou les conteneurs. C'est le plus souvent une machine physique, mais ce peut être une VM : conteneurs lancés dans une VM, virtualisation imbriquée. | [1](01-virtualisation-concepts-generaux.md) |
| **Hyperviseur** | Logiciel qui crée et exécute des machines virtuelles (type 1 sur le matériel, type 2 sur un OS). | [2](02-serveur-virtualise.md) |
| **IaaS / PaaS / SaaS** | Modèles de service cloud : infrastructure, plateforme ou logiciel fournis à la demande. | [6](06-cloud.md) |
| **IaC** | Infrastructure as Code : description de l'infrastructure dans des fichiers de code versionnés. | [8](08-infrastructure-as-code.md) |
| **Idempotence** | Propriété d'une opération qui produit le même résultat qu'on l'exécute une ou plusieurs fois. | [8](08-infrastructure-as-code.md) |
| **Image** | Modèle immuable, en couches, à partir duquel on crée des conteneurs. | [4](04-conteneurisation.md) |
| **Ingress** | Ressource Kubernetes qui expose des Services HTTP(S) vers l'extérieur, avec un routage par nom de domaine ou par chemin. | [5](05-orchestration.md) |
| **Inventaire** | Fichier Ansible (INI ou YAML) qui liste et regroupe les machines gérées. | [8](08-infrastructure-as-code.md) |
| **Invité** | Système d'exploitation ou application exécuté dans une VM ou un conteneur. | [1](01-virtualisation-concepts-generaux.md) |
| **LUN** | Volume de stockage en mode bloc présenté par un SAN à un serveur, qui le voit comme un disque local. | [3](03-stockage-virtualise.md) |
| **LVM** | Gestionnaire de volumes logiques de Linux (PV, VG, LV). | [3](03-stockage-virtualise.md) |
| **Migration à chaud** | Déplacement d'une VM d'un hôte à un autre sans interruption de service (par exemple VMware vMotion). | [1](01-virtualisation-concepts-generaux.md), [5](05-orchestration.md) |
| **Multi-tenant** | Partage d'une même infrastructure physique entre plusieurs clients (mutualisation), isolés les uns des autres. | [6](06-cloud.md) |
| **Namespace** | Sous Linux, mécanisme qui donne à un processus sa propre vue d'une ressource système ; sous Kubernetes, partition logique d'un cluster. | [4](04-conteneurisation.md), [5](05-orchestration.md) |
| **Observabilité** | Capacité à comprendre l'état interne d'un système à partir de ses métriques, de ses logs et de ses traces. | [9](09-monitoring.md) |
| **Orchestration** | Coordination automatisée de multiples tâches et ressources pour gérer un système complet. | [5](05-orchestration.md) |
| **Paravirtualisation** | Virtualisation dans laquelle le système invité, modifié ou doté de pilotes spécifiques (virtio), coopère avec l'hyperviseur au lieu de passer par du matériel émulé. | [2](02-serveur-virtualise.md) |
| **Playbook** | Fichier YAML Ansible qui associe des groupes de machines à une liste de tâches, chacune appelant un module. | [8](08-infrastructure-as-code.md) |
| **Pod** | Plus petite unité déployable de Kubernetes, regroupant un ou plusieurs conteneurs. | [5](05-orchestration.md) |
| **Provider** | Extension de Terraform qui fait le lien avec l'API d'un service (fournisseur de cloud, hyperviseur…). | [8](08-infrastructure-as-code.md) |
| **RAID** | Regroupement de disques pour améliorer performances et/ou tolérance aux pannes. Ce n'est pas une sauvegarde. | [3](03-stockage-virtualise.md) |
| **Registre** | Service qui stocke et distribue les images de conteneurs (Docker Hub, GitLab Container Registry, Harbor…). | [4](04-conteneurisation.md) |
| **ReplicaSet** | Ressource Kubernetes qui garantit qu'un nombre donné de pods identiques tourne en permanence. | [5](05-orchestration.md) |
| **SAN / NAS / DAS** | Stockage en mode bloc sur réseau dédié / stockage de fichiers en réseau / stockage local. | [3](03-stockage-virtualise.md) |
| **Scaling** | Ajustement de la capacité d'un système : vertical (machine plus puissante) ou horizontal (plus de machines). | [5](05-orchestration.md) |
| **SDN** | *Software-Defined Networking* : réseau dont le plan de contrôle est séparé du plan de données et centralisé dans un contrôleur logiciel. | [1](01-virtualisation-concepts-generaux.md) |
| **Serverless** | Approche où le code s'exécute sans gestion de l'infrastructure, qui s'adapte seule à la charge ; le FaaS (*Functions as a Service*) en est le cœur. | [6](06-cloud.md) |
| **Service** | Ressource Kubernetes qui donne un point d'accès stable (nom DNS et adresse IP) à un ensemble de pods et répartit le trafic entre eux. | [5](05-orchestration.md) |
| **SLA / SLO / SLI** | Contrat de service / objectif de service / indicateur de service. | [9](09-monitoring.md) |
| **Snapshot** | Instantané : copie de l'état d'un volume ou d'une VM à un moment donné, utile pour les sauvegardes et les tests. | [3](03-stockage-virtualise.md) |
| **State (Terraform)** | Fichier `terraform.tfstate` où Terraform mémorise l'infrastructure qu'il gère ; il peut contenir des secrets. | [8](08-infrastructure-as-code.md) |
| **Thin provisioning** | Allocation de l'espace de stockage au fur et à mesure de son utilisation réelle. | [3](03-stockage-virtualise.md) |
| **Vendor lock-in** | Enfermement propriétaire : dépendance aux services propres d'un fournisseur, qui rend difficile le départ vers un concurrent. | [6](06-cloud.md) |
| **VLAN** | Découpage logique d'un réseau physique en plusieurs réseaux isolés, au niveau 2 (norme IEEE 802.1Q). | [1](01-virtualisation-concepts-generaux.md) |
| **VM** | Machine virtuelle : ordinateur complet (processeur, mémoire, disques et réseau virtuels) fourni par un hyperviseur, avec son propre système d'exploitation. Ses instructions s'exécutent directement sur le processeur de l'hôte : ce n'est pas de l'émulation. | [1](01-virtualisation-concepts-generaux.md), [2](02-serveur-virtualise.md) |
| **Volume** | Espace de stockage persistant, géré par le moteur de conteneurs et monté dans un conteneur, dont la durée de vie dépasse celle du conteneur. | [4](04-conteneurisation.md) |
| **VPN** | Connexion chiffrée entre des sites ou des utilisateurs distants au travers d'Internet (IPsec, WireGuard, OpenVPN). | [1](01-virtualisation-concepts-generaux.md) |

---

[← 9. Monitoring](09-monitoring.md) · [Sommaire](README.md#sommaire)

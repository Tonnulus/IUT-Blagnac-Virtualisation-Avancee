# Présentation du module

## Objectifs

1. Approfondir les notions de virtualisation vues l'an dernier.
2. Comprendre comment optimiser l'utilisation d'un système par la mutualisation des ressources.
3. Appréhender les différentes méthodes de virtualisation.

## Compétences

1. Intégrer des solutions logicielles dans un environnement virtualisé.
2. Anticiper les résultats de diverses métriques d'un environnement virtualisé.

## Planning

| TD | Durée | Thème |
| --- | --- | --- |
| TD 1 | 4 h 30 | Introduction à la virtualisation avancée |
| TD 2 | 3 h | Infrastructure as Code |
| TD 3 | 3 h | Monitoring d'infrastructures virtualisées |
| TD 4 | 3 h | Orchestration |

## Évaluation

| Modalité | Part de la note |
| --- | --- |
| Évaluation sur table | 75 % |
| Contrôle continu, en TD | 25 % |

L'évaluation sur table (1 h, une feuille A4 manuscrite autorisée) porte sur les notions du cours, en particulier les encadrés « À retenir » du document *Les notions essentielles*, et sur ce qui a été pratiqué en TD.

Le contrôle continu repose sur un compte rendu individuel couvrant les quatre séances, accompagné des fichiers `seance1.txt` à `seance4.txt` produits par la commande `script`. Il est noté selon la grille suivante :

| Critère | Points |
| --- | --- |
| Progression : exercices réalisés au fil des quatre séances | 6 |
| Réponses aux questions : exactitude et justification | 8 |
| Preuves : sorties de commandes et captures d'écran pertinentes | 3 |
| Clarté et qualité de la rédaction | 3 |
| **Total** | **20** |

## Présentation des TD

### TD 1 : Introduction à la virtualisation avancée

➜ [Sujet du TD 1](td/td1-introduction-a-la-virtualisation-avancee.md)

<img src="images/presentation-du-module/image6.png" alt="Logo KVM" width="56" style="display: inline; width: 1.5cm; margin: 0 10pt 0 0"> <img src="images/presentation-du-module/image4.jpg" alt="Logo Docker" width="56" style="display: inline; width: 1.5cm; margin: 0">

#### Objectifs

Rappeler les notions évoquées dans le cours R4.A.08.

Définitions, avantages et inconvénients, exemples réels :

- virtualisation, paravirtualisation, émulation, hôte, invité, hyperviseurs de type 1 et 2 ;
- réseaux virtuels (VLAN, VPN, SDN) ;
- conteneurisation :
  - cgroups, namespaces, chroot, UnionFS, Netfilter ;
  - rôle de systemd vis-à-vis des conteneurs.

Utiliser Docker Compose :

- persistance des données (volumes) ;
- réseau interne/externe.

#### Exercices

Comparer les performances d'un serveur web sous Docker et sur VM :

- déployer un serveur web sur une VM avec Virt-Manager sous Linux ;
- déployer un serveur web avec Docker Compose ;
- comparer l'utilisation de la RAM et du CPU, le temps d'exécution et le temps de déploiement.

Déployer une application complexe avec Docker Compose :

- déployer OpenCTI (API, stockage, cache, connecteurs) ;
- gestion des interfaces réseau ;
- gestion de la persistance des données.

### TD 2 : Infrastructure as Code

➜ [Sujet du TD 2](td/td2-infrastructure-as-code.md)

<img src="images/presentation-du-module/image3.png" alt="Logo Ansible" width="56" style="display: inline; width: 1.5cm; margin: 0 10pt 0 0"> <img src="images/presentation-du-module/image5.png" alt="Logo Terraform" width="56" style="display: inline; width: 1.5cm; margin: 0">

#### Objectif

Aborder la notion d'Infrastructure as Code (IaC) et ses enjeux :

- DevOps ;
- cloud.

Automatiser le déploiement d'une application.

#### Exercices

- Utiliser Ansible pour automatiser le déploiement d'une application web.
- Utiliser Terraform pour déployer des conteneurs.

### TD 3 : Monitoring d'infrastructures virtualisées

➜ [Sujet du TD 3](td/td3-monitoring-d-infrastructures-virtualisees.md)

<img src="images/presentation-du-module/image7.png" alt="Logo Grafana" width="56" style="display: inline; width: 1.5cm; margin: 0 10pt 0 0"> <img src="images/presentation-du-module/image2.png" alt="Logo Prometheus" width="56" style="display: inline; width: 1.5cm; margin: 0">

#### Objectifs

Comprendre et manipuler les outils de monitoring :

- métriques avec Prometheus et Grafana (SNMP et Nagios sont présentés en cours).

Aller plus loin avec les concepts de partitionnement et d'isolation :

- partage des ressources entre l'hôte et le conteneur ;
- partitionnement LVM.

#### Exercices

Installer une stack de monitoring Grafana et Prometheus avec Docker Compose :

- monitorer son système avec cAdvisor et Node Exporter ;
- analyser ces métriques à l'aide de Grafana ;
- proposer des règles d'alerting.

Faire face à un incident de production (manque d'espace disque) :

- ajouter de l'espace disque à sa VM avec LVM.

### TD 4 : Orchestration

➜ [Sujet du TD 4](td/td4-orchestration.md)

<img src="images/presentation-du-module/image1.png" alt="Logo Kubernetes" width="56" style="display: inline; width: 1.5cm; margin: 0">

#### Objectif

Comprendre l'intérêt de l'orchestration :

- orchestration de VM (OpenStack, VMware) et de conteneurs (Kubernetes, Swarm), présentée en cours ;
- réplication ;
- autoscaling ;
- tolérance aux pannes (*fault tolerance*).

Introduire l'utilisation de Kubernetes :

- nodes, pods, services.

#### Exercice

Déployer une application web à l'aide de Minikube :

- utilisation de Minikube et de kubectl ;
- mise à l'échelle manuelle puis automatique (HPA) ;
- remplacement automatique d'un pod supprimé.

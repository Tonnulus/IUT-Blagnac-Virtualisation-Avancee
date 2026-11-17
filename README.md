# Virtualisation avancée — IUT de Blagnac

Supports du module **R5.A.09 Virtualisation avancée** (BUT, semestre 5, IUT de Blagnac) : présentation du module, cours de référence et sujets des quatre TD.

Chaque document se lit directement sur GitHub (Markdown) et existe aussi en PDF, mis en page pour l'impression.

## Par où commencer

1. Lire la [présentation du module](presentation-du-module.md) : objectifs, planning, modalités d'évaluation.
2. Garder sous la main [Les notions essentielles](cours/les-notions-essentielles.md), le cours de référence. Ses encadrés « À retenir » forment le socle attendu à l'évaluation.
3. Suivre les TD dans l'ordre : chacun s'appuie sur le précédent.

## Contenu

### Présentation du module

| Document | Lire en ligne | PDF |
| --- | --- | --- |
| Présentation du module | [Markdown](presentation-du-module.md) | [PDF](pdf/00-presentation-du-module.pdf) |

### Cours

| Document | Thèmes | Lire en ligne | PDF |
| --- | --- | --- | --- |
| Les notions essentielles | Virtualisation, stockage (RAID, LVM), conteneurs, orchestration, cloud, DevOps, Infrastructure as Code, monitoring | [Markdown](cours/les-notions-essentielles.md) | [PDF](pdf/cours-les-notions-essentielles.pdf) |

### Travaux dirigés

| TD | Durée | Thème | Outils | Lire en ligne | PDF |
| --- | --- | --- | --- | --- | --- |
| TD 1 | 4 h 30 | Introduction à la virtualisation avancée | Virt-Manager (KVM), Docker, Docker Compose, OpenCTI | [Markdown](td/td1-introduction-a-la-virtualisation-avancee.md) | [PDF](pdf/td1-introduction-a-la-virtualisation-avancee.pdf) |
| TD 2 | 3 h | Infrastructure as Code | Ansible, Terraform | [Markdown](td/td2-infrastructure-as-code.md) | [PDF](pdf/td2-infrastructure-as-code.pdf) |
| TD 3 | 3 h | Monitoring d'infrastructures virtualisées | Prometheus, Grafana, cAdvisor, Node Exporter, LVM | [Markdown](td/td3-monitoring-d-infrastructures-virtualisees.md) | [PDF](pdf/td3-monitoring-d-infrastructures-virtualisees.pdf) |
| TD 4 | 3 h | Orchestration | Minikube, kubectl (Kubernetes) | [Markdown](td/td4-orchestration.md) | [PDF](pdf/td4-orchestration.pdf) |

## Prérequis pour les TD

- Un poste Linux avec les droits administrateur (les TD sont écrits pour Ubuntu 22.04, l'environnement des salles de l'IUT).
- Docker et Docker Compose : leur installation est détaillée au début du [TD 1](td/td1-introduction-a-la-virtualisation-avancee.md#préparation-du-poste).
- Un compte Docker Hub (gratuit).
- Une virtualisation matérielle activée (KVM) pour les exercices sur machine virtuelle.

## Organisation du dépôt

```text
.
├── presentation-du-module.md   présentation du module
├── cours/                      cours de référence
├── td/                         sujets des TD 1 à 4
├── images/                     illustrations, un dossier par document
└── pdf/                        versions PDF de tous les documents
```

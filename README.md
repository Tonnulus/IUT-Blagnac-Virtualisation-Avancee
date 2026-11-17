# Virtualisation avancée — IUT de Blagnac

[![Licence : CC BY-NC-SA 4.0](https://img.shields.io/badge/licence-CC%20BY--NC--SA%204.0-lightgrey.svg)](https://creativecommons.org/licenses/by-nc-sa/4.0/deed.fr) [![Code : MIT](https://img.shields.io/badge/code-MIT-blue.svg)](LICENSE-CODE)

Supports du module **R5.A.09 Virtualisation avancée** (BUT Informatique, 3ᵉ année, semestre 5, IUT de Blagnac) : présentation du module, cours de référence et sujets des quatre TD.

Chaque document se lit directement sur GitHub (Markdown) et existe aussi en PDF, mis en page pour l'impression.

## Par où commencer

1. Lire la [présentation du module](presentation-du-module.md) : objectifs, planning, modalités d'évaluation.
2. Garder sous la main [Les notions essentielles](cours/README.md), le cours de référence. Ses encadrés « À retenir » forment le socle attendu à l'évaluation.
3. Suivre les TD dans l'ordre : chacun s'appuie sur le précédent. Chaque TD commence par des exercices théoriques, regroupés dans un QCM à compléter.

## Contenu

### Présentation du module

| Document | Lire en ligne | PDF |
| --- | --- | --- |
| Présentation du module | [Markdown](presentation-du-module.md) | [PDF](pdf/00-presentation-du-module.pdf) |

### Cours : les notions essentielles

[Introduction et sommaire](cours/README.md) · [PDF complet](pdf/cours-les-notions-essentielles.pdf)

| Chapitre | Thèmes |
| --- | --- |
| [1. Virtualisation, concepts généraux](cours/01-virtualisation-concepts-generaux.md) | Historique, virtualisation et émulation, avantages, domaines d'application |
| [2. Serveur virtualisé](cours/02-serveur-virtualise.md) | Hyperviseurs de type 1 et 2, paravirtualisation, conteneurs |
| [3. Stockage virtualisé](cours/03-stockage-virtualise.md) | Thin provisioning, DAS/SAN/NAS, RAID, LVM |
| [4. Conteneurisation](cours/04-conteneurisation.md) | Images, couches, volumes, réseau, Docker, Dockerfile, Docker Compose |
| [5. Orchestration](cours/05-orchestration.md) | Scaling, orchestration de VM et de conteneurs, Kubernetes |
| [6. Cloud](cours/06-cloud.md) | IaaS/PaaS/SaaS, modèles de déploiement, défis, tendances |
| [7. Le DevOps](cours/07-le-devops.md) | Principes, avantages et défis, outils |
| [8. Infrastructure as Code](cours/08-infrastructure-as-code.md) | Principes, Ansible, Terraform, CI/CD |
| [9. Monitoring](cours/09-monitoring.md) | Types de monitoring, outils, SLA/SLO/SLI |
| [10. Glossaire](cours/10-glossaire.md) | Termes clés du module |

### Travaux dirigés

| TD | Durée | Thème | Outils | Sujet | PDF | QCM |
| --- | --- | --- | --- | --- | --- | --- |
| TD 1 | 4 h 30 | Introduction à la virtualisation avancée | Virt-Manager (KVM), Docker, Docker Compose, OpenCTI | [Markdown](td/td1-introduction-a-la-virtualisation-avancee.md) | [PDF](pdf/td1-introduction-a-la-virtualisation-avancee.pdf) | [QCM](td/td1-qcm.md) |
| TD 2 | 3 h | Infrastructure as Code | Ansible, Terraform | [Markdown](td/td2-infrastructure-as-code.md) | [PDF](pdf/td2-infrastructure-as-code.pdf) | [QCM](td/td2-qcm.md) |
| TD 3 | 3 h | Monitoring d'infrastructures virtualisées | Prometheus, Grafana, cAdvisor, Node Exporter, LVM | [Markdown](td/td3-monitoring-d-infrastructures-virtualisees.md) | [PDF](pdf/td3-monitoring-d-infrastructures-virtualisees.pdf) | [QCM](td/td3-qcm.md) |
| TD 4 | 3 h | Orchestration | Minikube, kubectl (Kubernetes) | [Markdown](td/td4-orchestration.md) | [PDF](pdf/td4-orchestration.pdf) | [QCM](td/td4-qcm.md) |

## Prérequis pour les TD

- Un poste Linux avec les droits administrateur (les TD sont écrits pour Ubuntu 22.04, l'environnement des salles de l'IUT).
- Docker et Docker Compose : leur installation est détaillée au début du [TD 1](td/td1-introduction-a-la-virtualisation-avancee.md#préparation-du-poste).
- Un compte Docker Hub (gratuit).
- Une virtualisation matérielle activée (KVM) pour les exercices sur machine virtuelle.

## Organisation du dépôt

```text
.
├── presentation-du-module.md   présentation du module
├── cours/                      cours de référence, un fichier par chapitre
├── td/                         sujets et QCM des TD 1 à 4
├── images/                     illustrations, un dossier par document
├── pdf/                        versions PDF de tous les documents
├── LICENSE                     licence des supports (CC BY-NC-SA 4.0)
└── LICENSE-CODE                licence du code des exemples (MIT)
```

## Licence

© 2026 Antoine Dunan.

- **Supports de cours** (textes, schémas originaux, PDF) : [Creative Commons Attribution - Pas d'utilisation commerciale - Partage dans les mêmes conditions 4.0 International (CC BY-NC-SA 4.0)](https://creativecommons.org/licenses/by-nc-sa/4.0/deed.fr), texte complet dans [`LICENSE`](LICENSE). Vous pouvez les copier, les diffuser et les adapter, en citant l'auteur, sans usage commercial et en partageant vos versions modifiées sous la même licence.
- **Code des exemples** (commandes, Dockerfile, fichiers Docker Compose, playbooks Ansible, configurations Terraform, manifestes Kubernetes…) : [licence MIT](LICENSE-CODE), réutilisable librement.
- **Éléments tiers** : les logos (Docker, Kubernetes, Ansible, Terraform, Grafana, Prometheus, KVM…), les marques citées et les schémas repris de documentations officielles restent la propriété de leurs auteurs et ne sont pas couverts par ces licences.

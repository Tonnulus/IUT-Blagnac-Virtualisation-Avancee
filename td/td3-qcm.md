# TD 3 : QCM

*Exercices théoriques du [TD 3 : Monitoring d'infrastructures virtualisées](td3-monitoring-d-infrastructures-virtualisees.md).*

## Mode d'emploi

- Pour répondre, téléchargez ce fichier (bouton **Download raw file** de GitHub) ou copiez son contenu dans votre compte rendu.
- Cochez une case en remplaçant `[ ]` par `[x]`. Remplissez les cellules vides des tableaux.
- Sauf mention « plusieurs réponses possibles », une seule réponse est correcte.
- Pour vous aider : chapitres [3](../cours/03-stockage-virtualise.md) et [9](../cours/09-monitoring.md) du cours, ainsi que les liens donnés au début du TD.

## Exercice 1 : SLA, SLO, SLI et SRE

### 1.1 Associez chaque terme à sa définition, puis à son exemple

| N° | Terme |
| --- | --- |
| 1 | SLA (*Service Level Agreement*) |
| 2 | SLO (*Service Level Objective*) |
| 3 | SLI (*Service Level Indicator*) |

| Lettre | Définition |
| --- | --- |
| A | Métrique quantitative qui mesure une caractéristique précise du service, par exemple le taux de requêtes réussies ou la latence. |
| B | Contrat formel entre un prestataire et son client, qui fixe les engagements de qualité de service et les pénalités en cas de non-respect. |
| C | Objectif chiffré de performance ou de disponibilité, mesuré sur une période donnée. |

| Lettre | Exemple |
| --- | --- |
| D | Pour un service de messagerie : 95 % des messages doivent être livrés en moins de 10 secondes. |
| E | Pour une plateforme de vidéo : le temps de chargement des vidéos, mesuré au 95e centile. |
| F | Un hébergeur garantit par contrat une disponibilité de 99,9 % des sites de ses clients, sous peine de pénalités. |

**Vos réponses :**

| | 1 (SLA) | 2 (SLO) | 3 (SLI) |
| --- | --- | --- | --- |
| Définition | | | |
| Exemple | | | |

### 1.2 Un SLA garantit 99,9 % de disponibilité sur un mois de 30 jours. Quelle durée d'indisponibilité est tolérée au maximum ?

- [ ] Environ 4 minutes
- [ ] Environ 7 heures
- [ ] Environ 1 jour
- [ ] Environ 43 minutes

### 1.3 Par rapport à l'engagement contractuel du SLA, une équipe fixe généralement son SLO interne :

- [ ] plus exigeant, pour garder une marge
- [ ] moins exigeant, pour être sûre de l'atteindre
- [ ] strictement identique
- [ ] sans aucun lien avec le SLA

### 1.4 Quelles affirmations sur le SRE (*Site Reliability Engineering*) sont vraies ? *(plusieurs réponses possibles)*

- [ ] C'est une discipline née chez Google, qui applique les méthodes de l'ingénierie logicielle à l'exploitation.
- [ ] Elle vise à supprimer toute indisponibilité, quel qu'en soit le coût.
- [ ] Elle s'appuie sur l'automatisation des tâches répétitives.
- [ ] Le budget d'erreur est la part d'indisponibilité tolérée ; il arbitre entre nouvelles fonctionnalités et stabilité.
- [ ] Elle comprend la gestion des incidents et les analyses post-incident (*postmortems*).

## Exercice 2 : le monitoring avec Prometheus

### 2.1 Pour collecter les métriques, Prometheus fonctionne en mode :

- [ ] *push* : les cibles envoient leurs métriques au serveur
- [ ] *pull* : le serveur va chercher les métriques auprès des cibles
- [ ] *broadcast* : les cibles diffusent leurs métriques sur le réseau
- [ ] *batch* : les métriques sont importées une fois par jour

### 2.2 Sur quelle URL une cible expose-t-elle en général ses métriques ?

- [ ] `/status`
- [ ] `/health`
- [ ] `/metrics`
- [ ] `/api/v1/data`

### 2.3 Quel paramètre de `prometheus.yml` fixe la fréquence de collecte ?

- [ ] `scrape_interval`
- [ ] `evaluation_interval`
- [ ] `poll_rate`
- [ ] `refresh_period`

### 2.4 Quelle requête Prometheus envoie-t-il aux cibles à chaque collecte ?

- [ ] Une requête SNMP GET
- [ ] Une requête HTTP POST
- [ ] Une requête HTTP GET
- [ ] Une requête ICMP

### 2.5 Où Prometheus stocke-t-il les métriques collectées ?

- [ ] Dans une base de données relationnelle
- [ ] Dans des fichiers CSV
- [ ] Dans Grafana
- [ ] Dans une base de données de séries temporelles locale

### 2.6 Associez chaque composant à son rôle

| N° | Composant |
| --- | --- |
| 1 | Cibles |
| 2 | Exporteurs (par exemple Node Exporter) |
| 3 | Serveur Prometheus |
| 4 | Alertmanager |
| 5 | Grafana |

| Lettre | Rôle |
| --- | --- |
| A | Reçoit les alertes, les regroupe, les déduplique et les envoie vers les canaux de notification (e-mail, messagerie…). |
| B | Se connecte à Prometheus comme source de données et offre une interface de visualisation pour créer des tableaux de bord. |
| C | Applications ou services surveillés. |
| D | Collecte les métriques, les stocke et évalue les règles d'alerte. |
| E | Collecte des métriques spécifiques auprès d'une application, d'une base de données ou du système, et les expose dans un format compréhensible par Prometheus. |

**Vos réponses :**

| 1 | 2 | 3 | 4 | 5 |
| --- | --- | --- | --- | --- |
|   |   |   |   |   |

### 2.7 Quel composant évalue les règles d'alerte ?

- [ ] Alertmanager
- [ ] Le serveur Prometheus
- [ ] Grafana
- [ ] Node Exporter

## Exercice 3 : LVM

### 3.1 Parmi les éléments suivants, lesquels sont des systèmes de fichiers ? *(plusieurs réponses possibles)*

- [ ] ext4
- [ ] GPT
- [ ] NTFS
- [ ] XFS
- [ ] LVM
- [ ] FAT32
- [ ] RAID 5
- [ ] APFS

### 3.2 Quels sont les objectifs du partitionnement d'un disque ? *(plusieurs réponses possibles)*

- [ ] Isoler des ensembles de données, par exemple le système et les fichiers des utilisateurs
- [ ] Augmenter la capacité totale du disque
- [ ] Installer plusieurs systèmes d'exploitation sur le même disque
- [ ] Limiter l'impact d'une corruption de système de fichiers à une seule partition
- [ ] Réserver de l'espace à des usages particuliers (sauvegardes, fichiers temporaires…)

### 3.3 Vrai ou faux ?

| Affirmation | Vrai | Faux |
| --- | --- | --- |
| Une partition traditionnelle est une section contiguë d'un disque, dont la taille est difficile à modifier. | [ ] | [ ] |
| Avec LVM, un volume logique peut s'étendre sur plusieurs disques physiques. | [ ] | [ ] |
| On peut agrandir un volume logique et son système de fichiers ext4 à chaud, sans démontage. | [ ] | [ ] |
| Un système de fichiers XFS peut être réduit. | [ ] | [ ] |
| Un système de fichiers ext4 peut être réduit sans être démonté. | [ ] | [ ] |
| Un snapshot LVM est une copie complète du volume. | [ ] | [ ] |
| La commande `pvmove` déplace à chaud les données d'un disque physique vers un autre. | [ ] | [ ] |
| Ajouter un disque à LVM oblige à repartitionner les disques existants. | [ ] | [ ] |

### 3.4 Dans quel ordre s'empilent les abstractions de LVM, du disque vers le système de fichiers ?

- [ ] Groupe de volumes → volume physique → volume logique
- [ ] Volume physique → groupe de volumes → volume logique
- [ ] Volume logique → groupe de volumes → volume physique
- [ ] Volume physique → volume logique → groupe de volumes

### 3.5 Associez chaque abstraction de LVM à sa définition

| N° | Abstraction |
| --- | --- |
| 1 | Volume physique (PV) |
| 2 | Groupe de volumes (VG) |
| 3 | Volume logique (LV) |
| 4 | Extent |
| 5 | Snapshot |

| Lettre | Définition |
| --- | --- |
| A | Unité d'allocation de LVM, un bloc de taille fixe. |
| B | Partition ou disque entier ajouté à LVM. |
| C | Instantané qui fige l'état d'un volume à un instant donné, par copie à l'écriture. |
| D | Espace créé à partir d'un pool et utilisé comme une partition, mais redimensionnable. |
| E | Pool logique constitué d'un ou de plusieurs volumes physiques. |

**Vos réponses :**

| 1 | 2 | 3 | 4 | 5 |
| --- | --- | --- | --- | --- |
|   |   |   |   |   |

### 3.6 Quelle est la taille par défaut d'un extent ?

- [ ] 512 octets
- [ ] 4 Mio
- [ ] 1 Gio
- [ ] 64 Mio

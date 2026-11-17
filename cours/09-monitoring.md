*[Virtualisation avancée, les notions essentielles](README.md) › Chapitre 9*

# 9. Monitoring

Le **monitoring** (supervision) d'un système d'information consiste à surveiller en continu l'état et les performances des systèmes, des applications et des réseaux. Il vise à garantir leur disponibilité, leurs performances et leur fiabilité, en détectant et en résolvant rapidement les problèmes. La virtualisation, en mutualisant les ressources, renforce encore son importance : une VM trop gourmande peut dégrader toutes ses voisines.

## Pourquoi superviser

- **Détecter tôt les problèmes** : repérer une anomalie avant qu'elle ne devienne une panne réduit les interruptions de service et la gêne pour les utilisateurs.
- **Optimiser les performances** : le suivi des métriques clés révèle les goulets d'étranglement et les axes d'amélioration.
- **Planifier les capacités** : l'historique d'utilisation des ressources permet d'anticiper les besoins futurs et d'éviter la saturation.
- **Décider sur des faits** : les données de supervision fondent les décisions et l'allocation des ressources.

Le monitoring joue un rôle clé dans la boucle d'amélioration continue du DevOps.

Une chaîne de supervision comporte quatre étapes :

1. **Collecte** : les outils recueillent des données de sources variées (serveurs, équipements réseau, applications…).
2. **Stockage** : les données sont conservées dans une base adaptée, souvent une base de séries temporelles, pour être analysées.
3. **Analyse** : on recherche tendances, anomalies et problèmes. Des seuils déclenchent des alertes lorsqu'ils sont franchis.
4. **Visualisation** : les résultats sont présentés sous forme de tableaux de bord, de graphiques et de rapports.

On parle aujourd'hui d'**observabilité** pour désigner la capacité à comprendre l'état interne d'un système à partir de trois types de données : les métriques (mesures chiffrées), les logs (journaux d'événements) et les traces (le parcours d'une requête à travers les services).

## Les types de monitoring

### Monitoring des performances

Il suit les performances des systèmes, des applications et des réseaux : utilisation du processeur, de la mémoire, du réseau et des disques, temps de réponse des applications. Des alertes se déclenchent quand un seuil est dépassé, pour réagir avant l'interruption de service.

- **Prometheus et Grafana** : Prometheus est un système de supervision open source qui collecte et stocke des métriques. Grafana construit des tableaux de bord visuels et personnalisables à partir de ces données, pour suivre les performances en temps réel.
- **Nagios et SNMP** : Nagios est un outil de supervision open source qui vérifie l'état des hôtes et des services à l'aide de plugins. SNMP (*Simple Network Management Protocol*) est le protocole standard de supervision des équipements réseau et des périphériques : il remonte leur utilisation du processeur, de la mémoire, leur température, etc.

### Monitoring de la sécurité

Il surveille les activités suspectes ou malveillantes dans le système d'information : détection d'intrusions, analyse des journaux d'événements, détection comportementale des menaces. Ces outils sont ceux des SOC (*Security Operations Center*).

- **SIEM** (*Security Information and Event Management*) : des plateformes comme Splunk, Elastic Security ou IBM QRadar collectent, corrèlent et analysent les données de sécurité de multiples sources pour détecter menaces et anomalies.
- **IDS/IPS** (*Intrusion Detection / Prevention Systems*) : ils inspectent le trafic réseau à la recherche de signatures ou de comportements suspects ; un IPS peut en plus bloquer le trafic malveillant. Snort et Suricata sont des références open source.

### Monitoring des logs

La collecte et l'analyse des **journaux** produits par les différents composants permettent de retracer les événements passés, de diagnostiquer les problèmes et de dégager des tendances à long terme, comme les usages ou les erreurs récurrentes.

- **Elastic Stack** (ELK) : cette suite open source réunit Elasticsearch, qui indexe les logs, Logstash, qui les collecte et les transforme, et Kibana, qui fournit des tableaux de bord interactifs.
- **Splunk** : plateforme commerciale d'indexation, de recherche et de visualisation des logs, très répandue, qui détecte tendances et anomalies.
- **Grafana Loki** : alternative plus légère, intégrée à l'écosystème Grafana.

## Choisir ses métriques : USE, RED et golden signals

Un hôte expose des centaines de métriques, un cluster Kubernetes des milliers. Trois grilles de lecture aident à choisir celles qui comptent.

- **La méthode USE** (Brendan Gregg) s'applique aux **ressources** : processeur, mémoire, disque, réseau. Pour chacune, on suit :
  - l'**utilisation** (*Utilization*) : la part du temps où la ressource est occupée ;
  - la **saturation** (*Saturation*) : le travail en attente que la ressource ne peut pas absorber (file d'exécution du processeur, mémoire échangée sur le disque, file d'entrées-sorties) ;
  - les **erreurs** (*Errors*) : erreurs de disque, paquets perdus…
- **La méthode RED** (Tom Wilkie) s'applique aux **services** qui répondent à des requêtes, typiquement des microservices. Pour chacun, on suit :
  - le **débit** (*Rate*) : le nombre de requêtes par seconde ;
  - les **erreurs** (*Errors*) : le nombre de requêtes en échec ;
  - la **durée** (*Duration*) : le temps de réponse, exprimé en percentiles.
- **Les quatre signaux clés** (*golden signals*) de Google réunissent les deux points de vue : **latence**, **trafic**, **erreurs** et **saturation**.

| Méthode | S'applique à | Question posée | Exemple |
| --- | --- | --- | --- |
| USE | une ressource (hôte, VM, nœud, disque) | la ressource est-elle le goulot d'étranglement ? | file d'exécution du processeur supérieure au nombre de cœurs |
| RED | un service | les utilisateurs sont-ils bien servis ? | 99ᵉ percentile du temps de réponse supérieur à 500 ms |
| Golden signals | un service et ses ressources | les deux à la fois | latence, trafic, erreurs, saturation |

Une utilisation de 100 % n'est pas un problème en soi : c'est la **saturation** qui dégrade le service. À l'inverse, une utilisation moyenne modérée peut cacher des pics. Les métriques RED, vues par l'utilisateur, servent naturellement de SLI (voir [SLA, SLO et SLI](#sla-slo-et-sli)).

## Les métriques propres à la virtualisation

Dans une VM, le système invité ne voit que ses ressources virtuelles. Il peut se croire peu chargé alors qu'il attend l'hyperviseur, ou disposer de mémoire « libre » que l'hyperviseur lui a en réalité reprise. Il faut donc superviser à deux niveaux : dans l'invité **et** sur l'hôte.

Les requêtes PromQL de cette section s'appuient sur deux exportateurs de Prometheus utilisés au TD 3 : **node_exporter**, qui expose les métriques d'un hôte Linux (physique ou virtuel), et **cAdvisor**, qui expose celles des conteneurs.

### La surallocation

La **surallocation** (*overcommitment*) consiste à attribuer aux VM plus de ressources virtuelles que l'hôte n'en possède physiquement. On la mesure par un ratio : un hôte de 32 threads qui porte 40 VM de 4 vCPU distribue 160 vCPU, soit un ratio de 5 pour 1. Le pari est statistique, comme celui du thin provisioning (voir le [chapitre 3](03-stockage-virtualise.md)) : les VM sont rarement toutes chargées en même temps. Lorsqu'elles le sont, elles se disputent le matériel.

- **Processeur** : on accepte couramment des ratios de quelques vCPU par thread physique pour des charges générales, et l'on revient à 1 pour 1 pour des charges sensibles à la latence (bases de données, temps réel).
- **Mémoire** : la surallocation est plus risquée. L'hyperviseur dispose de plusieurs mécanismes pour récupérer de la mémoire : le **ballooning** (un pilote installé dans l'invité « gonfle » et rend à l'hyperviseur la mémoire qu'il occupe), la **déduplication de pages** identiques entre VM (KSM sous Linux) et, en dernier recours, l'**échange sur disque** (*swap*) de l'hôte, qui effondre les performances. En production, on évite généralement de surallouer la mémoire.

### Steal time et CPU ready

Lorsque l'hôte est saturé, un vCPU qui a du travail doit attendre que l'hyperviseur lui attribue un cœur physique. Ce temps d'attente porte deux noms selon l'endroit où on le mesure :

- le **steal time** (temps volé), mesuré depuis l'invité Linux sur KVM, Xen ou dans le cloud public : colonne `st` de `top` ou de `vmstat`, mode `steal` de node_exporter ;
- le **CPU ready**, mesuré par l'hyperviseur VMware et affiché dans vCenter.

Un steal time de quelques pour cent qui persiste est un signal d'alerte. Le symptôme typique : une application lente dans une VM dont le processeur n'est utilisé qu'à 60 %. Ajouter des vCPU à la VM n'y change rien, car le goulot d'étranglement est l'hôte : il faut déplacer la VM ou réduire la surallocation.

```promql
# Part du temps processeur volé par l'hyperviseur, par instance (en %)
avg by (instance) (rate(node_cpu_seconds_total{mode="steal"}[5m])) * 100
```

### La latence disque

Les VM d'un même hôte, ou d'un même stockage partagé (SAN, Ceph…), se partagent les mêmes disques. L'indicateur à suivre est la **latence** de chaque opération plus que le débit : une VM qui sature le stockage ralentit toutes les autres. Dans l'invité, `iostat -x` l'affiche (colonnes `r_await` et `w_await`, en millisecondes). Avec node_exporter :

```promql
# Latence moyenne d'une lecture, en secondes
rate(node_disk_read_time_seconds_total[5m]) / rate(node_disk_reads_completed_total[5m])
```

Pour fixer les idées : une lecture prend de l'ordre d'une fraction de milliseconde sur un SSD NVMe et d'une dizaine de millisecondes sur un disque dur. Une latence qui dépasse durablement quelques dizaines de millisecondes sur un stockage partagé indique une contention.

### Le voisin bruyant

Ces phénomènes ont une cause commune, le **voisin bruyant** (*noisy neighbor*) : une VM ou un conteneur qui consomme une ressource partagée au détriment des autres.

| Phénomène | Ce que l'on observe dans l'invité | Métrique à suivre | Où la lire |
| --- | --- | --- | --- |
| Surallocation du processeur | application lente, processeur peu chargé | steal time, CPU ready | `top`, node_exporter ; vCenter |
| Surallocation de la mémoire | lenteurs, activité d'échange | ballooning, swap de l'hôte | hyperviseur |
| Contention du stockage | latence élevée pour un faible débit | latence par opération | `iostat -x`, node_exporter ; hyperviseur, baie |
| Thin provisioning | rien, jusqu'au blocage de toutes les VM | taux de remplissage du pool | hyperviseur, baie |

### Et les conteneurs ?

Les conteneurs connaissent les mêmes phénomènes, à travers les limites que leur imposent les cgroups (voir le [chapitre 4](04-conteneurisation.md)) et que Kubernetes expose sous forme de *requests* et de *limits* (voir le [chapitre 5](05-orchestration.md#requests-et-limits)). Un conteneur qui atteint sa limite de processeur est **bridé** (*throttled*) : il ralentit sans erreur visible. Celui qui dépasse sa limite de mémoire est **tué** (`OOMKilled`) puis redémarré. Le bridage se suit avec les métriques de cAdvisor ; l'arrêt pour manque de mémoire apparaît dans `kubectl describe pod` :

```promql
# Part des périodes pendant lesquelles le conteneur a été bridé par sa limite de processeur
rate(container_cpu_cfs_throttled_periods_total[5m]) / rate(container_cpu_cfs_periods_total[5m])
```

### Anticiper plutôt que constater

Superviser, c'est aussi prévoir. Plutôt que d'alerter quand un disque est plein, on alerte quand la tendance indique qu'il le sera bientôt. La fonction PromQL `predict_linear` prolonge la tendance observée :

```promql
# Le système de fichiers racine sera-t-il plein dans les 24 heures, au rythme des 6 dernières heures ?
predict_linear(node_filesystem_avail_bytes{mountpoint="/"}[6h], 24 * 3600) < 0
```

Le même raisonnement s'applique au remplissage d'un pool en thin provisioning, à la croissance de la mémoire d'un service ou au nombre de vCPU distribués sur un cluster : c'est la planification des capacités.

## Les principaux outils

Il existe de très nombreux outils de supervision, propriétaires ou open source. Certains se spécialisent dans un domaine, d'autres se veulent des solutions globales. Comparons trois outils très présents en entreprise.

- **Prometheus** : système de supervision et d'alerte fondé sur les métriques. Il interroge activement ses cibles à intervalle régulier (modèle *pull*), stocke les données en séries temporelles et fournit un langage de requête, PromQL. Conçu pour les environnements *cloud native*, il est particulièrement adapté aux infrastructures dynamiques comme les conteneurs et Kubernetes.
- **Nagios** : outil de supervision plus traditionnel, fondé sur des vérifications actives de l'état et de la disponibilité des hôtes et des services. Il alerte en cas de dépassement de seuil et convient aux infrastructures plutôt statiques. Zabbix, très utilisé en France, couvre des besoins similaires.
- **Splunk** : plateforme de gestion de données qui va au-delà de la supervision. Elle collecte, indexe, recherche et visualise des données de toutes sources, en particulier les logs et les événements. Elle sert aussi bien à la sécurité qu'à l'analyse opérationnelle et à la supervision.

| | Prometheus | Nagios | Splunk |
| --- | --- | --- | --- |
| Données | métriques | état des services (OK, WARNING, CRITICAL) | logs et événements |
| Collecte | *pull* des métriques exposées | vérifications actives par plugins | agents et intégrations |
| Points forts | conteneurs, Kubernetes, PromQL | simplicité, disponibilité des services | recherche, corrélation, sécurité |
| Licence | open source | open source (Nagios Core) | commerciale |

Le choix dépend des besoins, de la complexité de l'infrastructure et des objectifs de supervision. En pratique, on combine souvent plusieurs outils.

## SLA, SLO et SLI

Trois notions permettent de formuler et de vérifier la qualité de service attendue. Elles ont des conséquences concrètes : ne pas tenir ses engagements peut entraîner des pénalités financières, voire la rupture d'un contrat.

### SLI, Service Level Indicator

Un **SLI** (indicateur de niveau de service) est une mesure quantitative d'un aspect du service : taux de disponibilité, temps de réponse, taux d'erreurs…

*Exemple :* pour une plateforme de streaming vidéo, le temps de chargement des vidéos. On le mesure de préférence en percentile (par exemple, le temps sous lequel se chargent 95 % des vidéos) plutôt qu'en moyenne, qui masque les mauvaises expériences.

### SLO, Service Level Objective

Un **SLO** (objectif de niveau de service) est la cible que l'on fixe pour un SLI, sur une période donnée. C'est en général un objectif interne, plus exigeant que l'engagement contractuel, pour garder une marge.

*Exemple :* pour une messagerie, 95 % des messages doivent être distribués en moins de 10 secondes, sur une période glissante de 30 jours.

### SLA, Service Level Agreement

Un **SLA** (accord de niveau de service) est le contrat passé entre un prestataire et son client. Il fixe les engagements de qualité et de performance, et les pénalités prévues en cas de manquement.

*Exemple :* un hébergeur garantit une disponibilité de 99,9 % par mois pour les sites de ses clients. Au-delà de 0,1 % d'indisponibilité (environ 43 minutes sur un mois de 30 jours), il manque à son engagement et doit indemniser ses clients.

| Disponibilité | Indisponibilité tolérée par an | par mois (30 jours) |
| --- | --- | --- |
| 99 % | 3 j 15 h 36 min | 7 h 12 min |
| 99,9 % | 8 h 45 min 36 s | 43 min 12 s |
| 99,95 % | 4 h 22 min 48 s | 21 min 36 s |
| 99,99 % | 52 min 34 s | 4 min 19 s |

L'écart entre 100 % et le SLO forme le **budget d'erreur** : la quantité d'indisponibilité que l'on peut « dépenser », par exemple en mises en production risquées, avant de devoir privilégier la stabilité.

En résumé :

- le SLI est ce que l'on mesure ;
- le SLO est l'objectif que l'on se fixe pour cette mesure ;
- le SLA est l'engagement contractuel pris envers le client, assorti de pénalités.

Les équipes d'exploitation s'en servent pour :

- définir des objectifs de performance clairs et mesurables (SLO) pour chaque service ;
- mesurer les performances réelles (SLI) et les comparer aux objectifs ;
- vérifier que les niveaux de service promis (SLA) sont tenus et, si nécessaire, prendre des mesures correctives.

> **À retenir**
>
> - Le monitoring suit quatre étapes : collecte, stockage, analyse (avec alertes), visualisation.
> - Métriques, logs et traces forment les trois piliers de l'observabilité.
> - USE pour les ressources (utilisation, saturation, erreurs), RED pour les services (débit, erreurs, durée) ; les golden signals réunissent les deux.
> - En environnement virtualisé, on supervise l'invité et l'hôte : steal time ou CPU ready (surallocation du processeur), ballooning et swap (mémoire), latence disque (stockage partagé).
> - Anticiper, c'est alerter sur une tendance (`predict_linear`) plutôt que sur un seuil déjà franchi.
> - Prometheus pour les métriques et le cloud native, Nagios pour la disponibilité des services, Splunk et ELK pour les logs et la sécurité.
> - SLI = mesure, SLO = objectif, SLA = contrat. 99,9 % de disponibilité autorise environ 8 h 45 min d'arrêt par an.

**Pour aller plus loin**

- [Prometheus, présentation](https://prometheus.io/docs/introduction/overview/)
- [Brendan Gregg, « The USE Method »](https://www.brendangregg.com/usemethod.html)
- [Grafana Labs, « The RED Method: how to instrument your services »](https://grafana.com/blog/2018/08/02/the-red-method-how-to-instrument-your-services/)
- [Google, *Site Reliability Engineering*, chapitre « Monitoring Distributed Systems »](https://sre.google/sre-book/monitoring-distributed-systems/) (les quatre golden signals)
- [Google Cloud, « SRE fundamentals: SLIs, SLAs and SLOs »](https://cloud.google.com/blog/products/devops-sre/sre-fundamentals-slis-slas-and-slos)
- [Google, *Site Reliability Engineering*, chapitre « Service Level Objectives »](https://sre.google/sre-book/service-level-objectives/)

---

[← 8. Infrastructure as Code](08-infrastructure-as-code.md) · [Sommaire](README.md#sommaire) · [10. Glossaire →](10-glossaire.md)

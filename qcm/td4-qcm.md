# TD 4 : QCM

*Exercices théoriques du [TD 4 : Orchestration](../td/td4-orchestration.md).*

## Mode d'emploi

- Pour répondre, téléchargez ce fichier (bouton **Download raw file** de GitHub) ou copiez son contenu dans votre compte rendu.
- Cochez une case en remplaçant `[ ]` par `[x]`. Remplissez les cellules vides des tableaux.
- Sauf mention « plusieurs réponses possibles », une seule réponse est correcte.
- Pour vous aider : chapitre [5](../cours/05-orchestration.md) du cours, la [documentation de Kubernetes](https://kubernetes.io/fr/docs/concepts/) et les liens donnés au début du TD.

## Exercice 1 : définitions

### 1.1 Qu'est-ce que l'orchestration, dans le contexte de la virtualisation ?

- [ ] La division d'un serveur physique en plusieurs machines virtuelles
- [ ] L'exécution d'un script de démarrage sur une machine virtuelle
- [ ] L'automatisation et la coordination du déploiement, du placement, de la mise en réseau, de la mise à l'échelle, de la surveillance et de la reprise après panne d'applications réparties sur de nombreuses VM ou de nombreux conteneurs
- [ ] La collecte des métriques des machines virtuelles dans un tableau de bord

### 1.2 Avec un orchestrateur, l'administrateur décrit :

- [ ] l'état souhaité, que l'orchestrateur atteint et maintient en permanence
- [ ] la liste des commandes à exécuter, dans l'ordre, sur chaque serveur
- [ ] uniquement le nombre de serveurs physiques du cluster
- [ ] l'adresse IP de chaque conteneur

### 1.3 Associez chaque notion à sa définition

| N° | Notion |
| --- | --- |
| 1 | Réplication |
| 2 | Autoscaling |
| 3 | Tolérance aux pannes |

| Lettre | Définition |
| --- | --- |
| A | Capacité d'un système à maintenir un niveau de service acceptable malgré des défaillances matérielles, logicielles ou réseau. |
| B | Exécution simultanée de plusieurs instances identiques d'une application ou d'un service. |
| C | Ajustement automatique des ressources d'une application en fonction de la charge de travail. |

**Vos réponses :**

| 1 | 2 | 3 |
| --- | --- | --- |
|   |   |   |

### 1.4 Le plus souvent, l'autoscaling :

- [ ] augmente le processeur et la mémoire de chaque instance (mise à l'échelle verticale)
- [ ] ajoute ou retire des instances (mise à l'échelle horizontale)
- [ ] ajoute des nœuds physiques au centre de données
- [ ] redémarre l'application à intervalle régulier

### 1.5 Quelles affirmations sur Kubernetes sont vraies ? *(plusieurs réponses possibles)*

- [ ] C'est une plateforme open source, conçue à l'origine par Google et maintenue aujourd'hui par la CNCF.
- [ ] Son nom est souvent abrégé en K8s.
- [ ] Il exige Docker Engine sur chaque nœud.
- [ ] Il redémarre ou remplace les conteneurs défaillants.
- [ ] Il effectue des mises à jour progressives et des retours en arrière.

### 1.6 Chaque composant fait-il partie du **plan de contrôle (C)** ou s'exécute-t-il sur **chaque nœud (N)** ?

| Composant | C ou N |
| --- | --- |
| kubelet | |
| kube-apiserver (API Server) | |
| etcd | |
| kube-proxy | |
| kube-scheduler (Scheduler) | |
| Container runtime (containerd, CRI-O) | |
| kube-controller-manager (Controller Manager) | |

### 1.7 Associez chaque composant à son rôle

| N° | Composant |
| --- | --- |
| 1 | API Server |
| 2 | Scheduler |
| 3 | Controller Manager |
| 4 | etcd |
| 5 | kubelet |
| 6 | kube-proxy |
| 7 | Container runtime |

| Lettre | Rôle |
| --- | --- |
| A | Base de données clé-valeur distribuée qui stocke la configuration du cluster et l'état de chaque objet. |
| B | Agent qui s'assure que les conteneurs des pods affectés à son nœud sont en cours d'exécution et en bonne santé. |
| C | Expose l'API Kubernetes, utilisée par `kubectl`, par les composants internes et par d'autres systèmes. |
| D | Exécute les conteneurs. |
| E | Exécute les boucles de contrôle qui font correspondre l'état réel du cluster à l'état souhaité. |
| F | Maintient les règles réseau du nœud qui redirigent et répartissent le trafic d'un Service vers ses pods. |
| G | Affecte les pods aux nœuds, selon les ressources disponibles et les contraintes définies. |

**Vos réponses :**

| 1 | 2 | 3 | 4 | 5 | 6 | 7 |
| --- | --- | --- | --- | --- | --- | --- |
|   |   |   |   |   |   |   |

### 1.8 Quel est le seul composant qui accède directement à etcd ?

- [ ] Le kubelet
- [ ] Le Scheduler
- [ ] L'API Server
- [ ] `kubectl`

### 1.9 Qui assure la communication directe entre les pods ?

- [ ] Le plugin réseau (CNI)
- [ ] kube-proxy
- [ ] etcd
- [ ] Le Scheduler

## Exercice 2 : QCM Kubernetes

### 2.1 Parmi les tâches suivantes, lesquelles peuvent être effectuées par Kubernetes ? *(plusieurs réponses possibles)*

- [ ] Déployer votre application
- [ ] Faire évoluer votre application de manière dynamique en fonction de la demande
- [ ] Auto-réparation de l'application en cas de panne
- [ ] Effectuer des mises à jour et des retours en arrière sans temps d'arrêt pour votre application

### 2.2 Les fichiers manifestes sont rédigés en : *(plusieurs réponses possibles)*

- [ ] XML
- [ ] YAML
- [ ] JSON
- [ ] HTML

### 2.3 Qu'est-ce que l'unité atomique d'ordonnancement dans Kubernetes ?

- [ ] Node
- [ ] Pod
- [ ] Container
- [ ] VM

### 2.4 Un pod Kubernetes ne peut avoir qu'un seul conteneur en cours d'exécution.

- [ ] Vrai
- [ ] Faux

### 2.5 Que se passe-t-il lorsqu'un pod géré par un Deployment meurt ?

- [ ] Kubernetes le restaure.
- [ ] Kubernetes en démarre un nouveau à sa place.
- [ ] Kubernetes soulève une erreur.
- [ ] Aucune de ces réponses.

### 2.6 Quel est l'objectif des Services ?

- [ ] Ils mettent en œuvre un contrôleur et une boucle de surveillance qui observent constamment le cluster, en s'assurant que l'état actuel correspond à l'état souhaité.
- [ ] Ils assurent une mise en réseau fiable d'un ensemble de pods.
- [ ] Ils fournissent un environnement pour l'exécution des conteneurs.
- [ ] Aucune de ces réponses.

### 2.7 Laquelle des affirmations suivantes est vraie ?

- [ ] Les pods ne peuvent héberger qu'un seul conteneur.
- [ ] Un pod peut héberger plusieurs conteneurs.
- [ ] Les conteneurs ne peuvent héberger qu'un seul pod.
- [ ] Les conteneurs peuvent héberger plus d'un pod.

### 2.8 Tous les conteneurs d'un pod partagent lesquels des éléments suivants ? *(plusieurs réponses possibles)*

- [ ] Un nom d'hôte
- [ ] Une adresse IP
- [ ] Un espace d'adressage de la mémoire
- [ ] Un volume

### 2.9 Deux conteneurs du même pod ne peuvent pas utiliser le même port.

- [ ] Vrai
- [ ] Faux

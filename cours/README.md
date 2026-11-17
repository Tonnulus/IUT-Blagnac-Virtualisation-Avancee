<p align="center">
  <img src="../images/les-notions-essentielles/image1.jpg" alt="Un nuage posé dans un cornet de glace, sur fond de ciel bleu" width="320">
</p>

# Virtualisation avancée, les notions essentielles

## Introduction

Ce document rassemble les notions à maîtriser pour suivre le module de virtualisation avancée : virtualisation des serveurs et du stockage, conteneurs, orchestration, cloud, DevOps, Infrastructure as Code et monitoring. Chaque chapitre se termine par un encadré À retenir et une sélection de lectures pour approfondir. Les encadrés « À retenir » forment le socle attendu à l'évaluation ; le reste du texte l'explique et l'approfondit. Un glossaire en fin de document reprend les termes clés.

**À l'issue de ce cours, vous saurez :**

- expliquer ce qu'est la virtualisation, la distinguer de l'émulation et citer ses domaines d'application ;
- comparer les hyperviseurs de type 1 et de type 2, la paravirtualisation et la conteneurisation ;
- choisir une architecture de stockage (DAS, SAN, NAS), un niveau de RAID et organiser des volumes avec LVM ;
- construire et lancer des conteneurs Docker, puis décrire comment Kubernetes les orchestre et leur alloue des ressources (requests, limits) ;
- situer les modèles de service (IaaS, PaaS, SaaS) et de déploiement du cloud ;
- expliquer la démarche DevOps et le rôle de l'Infrastructure as Code (Ansible, Terraform, CI/CD, GitOps) ;
- définir une stratégie de monitoring, anticiper le comportement des métriques d'un environnement virtualisé (surallocation, steal time, latence disque) et formuler des objectifs de service (SLA, SLO, SLI).

## Sommaire

- [Introduction](#introduction)

1. [Virtualisation, concepts généraux](01-virtualisation-concepts-generaux.md)
   - [Historique](01-virtualisation-concepts-generaux.md#historique)
   - [Virtualisation et émulation](01-virtualisation-concepts-generaux.md#virtualisation-et-émulation)
   - [Avantages et inconvénients](01-virtualisation-concepts-generaux.md#avantages-et-inconvénients)
   - [Les domaines d'application](01-virtualisation-concepts-generaux.md#les-domaines-dapplication)
2. [Serveur virtualisé](02-serveur-virtualise.md)
   - [Hyperviseurs de type 1](02-serveur-virtualise.md#hyperviseurs-de-type-1)
   - [Hyperviseurs de type 2](02-serveur-virtualise.md#hyperviseurs-de-type-2)
   - [Paravirtualisation](02-serveur-virtualise.md#paravirtualisation)
   - [Isolation par conteneurs](02-serveur-virtualise.md#isolation-par-conteneurs)
   - [Synthèse](02-serveur-virtualise.md#synthèse)
3. [Stockage virtualisé](03-stockage-virtualise.md)
   - [Disque virtuel et thin provisioning](03-stockage-virtualise.md#disque-virtuel-et-thin-provisioning)
   - [Les architectures de stockage](03-stockage-virtualise.md#les-architectures-de-stockage)
   - [RAID](03-stockage-virtualise.md#raid)
   - [LVM](03-stockage-virtualise.md#lvm)
4. [Conteneurisation](04-conteneurisation.md)
   - [Introduction à la conteneurisation](04-conteneurisation.md#introduction-à-la-conteneurisation)
   - [Fonctionnement de la conteneurisation](04-conteneurisation.md#fonctionnement-de-la-conteneurisation)
   - [Les concepts importants](04-conteneurisation.md#les-concepts-importants)
   - [Docker en pratique](04-conteneurisation.md#docker-en-pratique)
5. [Orchestration](05-orchestration.md)
   - [Automatisation et orchestration](05-orchestration.md#automatisation-et-orchestration)
   - [Le scaling](05-orchestration.md#le-scaling)
   - [Orchestration de machines virtuelles](05-orchestration.md#orchestration-de-machines-virtuelles)
   - [Orchestration de conteneurs](05-orchestration.md#orchestration-de-conteneurs)
   - [Kubernetes](05-orchestration.md#kubernetes)
6. [Cloud](06-cloud.md)
   - [Introduction au cloud](06-cloud.md#introduction-au-cloud)
   - [Les modèles de service](06-cloud.md#les-modèles-de-service)
   - [Les modèles de déploiement](06-cloud.md#les-modèles-de-déploiement)
   - [Les défis du cloud](06-cloud.md#les-défis-du-cloud)
   - [Tendances](06-cloud.md#tendances)
7. [Le DevOps](07-le-devops.md)
   - [Les principes fondamentaux](07-le-devops.md#les-principes-fondamentaux)
   - [Avantages et défis](07-le-devops.md#avantages-et-défis)
   - [Outils et pratiques](07-le-devops.md#outils-et-pratiques)
8. [Infrastructure as Code](08-infrastructure-as-code.md)
   - [Introduction à l'IaC](08-infrastructure-as-code.md#introduction-à-liac)
   - [Les principes clés](08-infrastructure-as-code.md#les-principes-clés)
   - [Panorama des outils](08-infrastructure-as-code.md#panorama-des-outils)
   - [Ansible en pratique](08-infrastructure-as-code.md#ansible-en-pratique)
   - [Terraform en pratique](08-infrastructure-as-code.md#terraform-en-pratique)
   - [CI/CD](08-infrastructure-as-code.md#cicd)
   - [GitOps](08-infrastructure-as-code.md#gitops)
9. [Monitoring](09-monitoring.md)
   - [Pourquoi superviser](09-monitoring.md#pourquoi-superviser)
   - [Les types de monitoring](09-monitoring.md#les-types-de-monitoring)
   - [Choisir ses métriques : USE, RED et golden signals](09-monitoring.md#choisir-ses-métriques--use-red-et-golden-signals)
   - [Les métriques propres à la virtualisation](09-monitoring.md#les-métriques-propres-à-la-virtualisation)
   - [Les principaux outils](09-monitoring.md#les-principaux-outils)
   - [SLA, SLO et SLI](09-monitoring.md#sla-slo-et-sli)
10. [Glossaire](10-glossaire.md)

---

[Commencer la lecture : 1. Virtualisation, concepts généraux →](01-virtualisation-concepts-generaux.md)

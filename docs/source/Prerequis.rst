---
title: Prerequis
---

Prerequis
===

Horizon n'a qu'une seule véritable dépendance : un serveur PostgreSQL qu'il utilise pour stocker les données qui ont été traitées et ingérées depuis Stellar Core. **Horizon nécessite la version PostgreSQL >= 9.5**.

En ce qui concerne la configuration système requise, il y a quelques éléments principaux à garder à l'esprit. À partir de la version 2.0, Horizon doit être exécuté en tant que service autonome. Une version complète d'Horizon comprend trois fonctions :

1. **ingérer des données** _depuis_ le réseau Stellar décentralisé,
1. **soumettre des transactions** _au_ réseau, et
1. **servir** les demandes d'API

#### Conditions

Au minimum, votre type de stockage sur disque **doit** être un SSD (par exemple, NVMe, stockage en attachement direct) et vos E/S **doivent** gérer > 15 000 iops (opérations d'E/S par seconde). 
Le tableau suivant détaille les spécifications matérielles pour l'ingestion à différents niveaux de rétention et niveaux de performances.

| Component| retention period | retention period |
| :-- | :-- | :-- |
|  | **30 days** | **90 days** | **Full History** |
| **Parallel worker count**<br/>(est. ingestion time) | 6 workers (1 day) | 10 workers (1 day) | 20+ workers (2 days) |
| **Horizon** | **CPU**: 10 cores (min: 6) <br/>**RAM**: 64 GB (min: 32)<br/> | **CPU**: 16 (min: 8)<br/> **RAM**: 128 GB (64)<br/> | **CPU**: 16 (10)<br/> **RAM**: 512 GB (256) |
| **Database** | **CPU**: 16 cores (min: 8)<br/>**RAM**: 64 GB (min: 32GB)<br/>**Storage**: 2 TB<br/>**IOPS**: 20K (min: 15K) | **CPU**: 16 (12)<br/>**RAM**: 128 GB (64)<br/>**Storage**: 4 TB<br/>**IOPS**: 20K (15K) | **CPU**: 64 (32)<br/>**RAM**: 512 GB (256)<br/>**Storage**: 10 TB<br/>**IOPS**: 20k (15k) |
| **Storage**<br/>(all same) |  | **SSD** (NVMe, Direct Attached Storage preferred) |  |
| **AWS**<br/>(reference) | **Captive Core**: `m5.2xlarge`<br/>**Database**: `r5.2xlarge` | **Captive Core**: `m5.4xlarge`<br/>**DB**: `r5.4xlarge` | **Captive Core**: `c5.2xlarge` (x2)<br/>**DB**: `r5.16xlarge` (ro)<br/>`r5.8xlarge` (rw) |

### Real-Time Ingestion

Dans ce scénario, l'objectif est simplement de rester synchronisé avec le réseau Stellar pour les opérations quotidiennes.


#### Conditions

Le tableau suivant détaille les exigences

| Category | Private Instance | Enterprise Public Instance |
| :-- | :-- | :-- |
| **Compute** | Both **API Service** + **Captive Core**:<br/>**CPU**: 4<br/>**RAM**: 32 GB | **API Service**<br/>**CPU**: 4<br/>**RAM**: 8 GB<br/>N instances, load balanced <br/><br/> **Captive Core**<br/>**CPU**: 8<br/>**RAM**: 256 GB<br/>2 instances for redundancy |
| **Database** | **CPU**: 4<br/> **RAM**: 32 GB<br/> **IOPS**: 7k (min: 2.5k) | **CPU**: 32 - 64<br/> **RAM**: 256 - 512 GB<br/> **IOPS**: 20k (min: 15k)<br/> 2 HA instances: 1RO, 1RW |
| **Storage** (SSD) | depends on retention period | 10 TB |
| **AWS** (reference) | **API Service + Captive Core**<br/>`m5.2xlarge`<br/><br/>**Database**<br/>`r5.2xlarge` (ro)<br/>`r5.xlarge` (rw) | **API Service**<br/>`c5.xlarge` (_n_) <br/><br/>**Captive Core**<br/>`c5.2xlarge` (x2)<br/><br/>**Database** `r5.16xlarge` (ro)<br/>`r5.8xlarge` (rw) |



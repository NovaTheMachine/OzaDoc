Prerequis
=========

Horizon n’a qu’une seule véritable dépendance : un serveur PostgreSQL
qu’il utilise pour stocker les données qui ont été traitées et ingérées
depuis Stellar Core. **Horizon nécessite la version PostgreSQL >= 9.5**.

En ce qui concerne la configuration système requise, il y a quelques
éléments principaux à garder à l’esprit. À partir de la version 2.0,
Horizon doit être exécuté en tant que service autonome. Une version
complète d’Horizon comprend trois fonctions :

1. **ingérer des données** *depuis* le réseau Stellar décentralisé,
2. **soumettre des transactions** *au* réseau, et 1. **servir** les
   demandes d’API

Conditions
^^^^^^^^^^

Au minimum, votre type de stockage sur disque **doit** être un SSD (par
exemple, NVMe, stockage en attachement direct) et vos E/S **doivent**
gérer > 15 000 iops (opérations d’E/S par seconde). Le tableau suivant
détaille les spécifications matérielles pour l’ingestion à différents
niveaux de rétention et niveaux de performances.

+-----------------------+-----------------------+-----------------------+
| Component             | retention period      | retention period      |
+=======================+=======================+=======================+
|                       | **30 days**           | **90 days**           |
+-----------------------+-----------------------+-----------------------+
| **Parallel worker     | 6 workers (1 day)     | 10 workers (1 day)    |
| count**\ (est.        |                       |                       |
| ingestion time)       |                       |                       |
+-----------------------+-----------------------+-----------------------+
| **Horizon**           | **CPU**: 10 cores     | **CPU**: 16 (min: 8)  |
|                       | (min: 6) \ **RAM**:   | **RAM**: 128 GB (64)  |
|                       | 64 GB (min: 32)       |                       |
+-----------------------+-----------------------+-----------------------+
| **Database**          | **CPU**: 16 cores     | **CPU**: 16           |
|                       | (min: 8)\ **RAM**: 64 | (12)\ **RAM**: 128 GB |
|                       | GB (min:              | (64)\ **Storage**: 4  |
|                       | 32GB)\ **Storage**: 2 | TB\ **IOPS**: 20K     |
|                       | TB\ **IOPS**: 20K     | (15K)                 |
|                       | (min: 15K)            |                       |
+-----------------------+-----------------------+-----------------------+
| **Storage**\ (all     |                       | **SSD** (NVMe, Direct |
| same)                 |                       | Attached Storage      |
|                       |                       | preferred)            |
+-----------------------+-----------------------+-----------------------+
| **AWS**\ (reference)  | **Captive Core**:     | **Captive Core**:     |
|                       | ``m5.2xlar            | ``m5                  |
|                       | ge``\ \ **Database**: | .4xlarge``\ \ **DB**: |
|                       | ``r5.2xlarge``        | ``r5.4xlarge``        |
+-----------------------+-----------------------+-----------------------+

Real-Time Ingestion
~~~~~~~~~~~~~~~~~~~

Dans ce scénario, l’objectif est simplement de rester synchronisé avec
le réseau Stellar pour les opérations quotidiennes.

.. _conditions-1:

Conditions
^^^^^^^^^^

Le tableau suivant détaille les exigences

+-----------------------+-----------------------+-----------------------+
| Category              | Private Instance      | Enterprise Public     |
|                       |                       | Instance              |
+=======================+=======================+=======================+
| **Compute**           | Both **API Service**  | **API                 |
|                       | + **Captive           | Service**\ \ **CPU**: |
|                       | Core**:\ **CPU**:     | 4\ **RAM**: 8 GBN     |
|                       | 4\ **RAM**: 32 GB     | instances, load       |
|                       |                       | balanced **Captive    |
|                       |                       | Core**\ \ **CPU**:    |
|                       |                       | 8\ **RAM**: 256 GB2   |
|                       |                       | instances for         |
|                       |                       | redundancy            |
+-----------------------+-----------------------+-----------------------+
| **Database**          | **CPU**: 4 **RAM**:   | **CPU**: 32 - 64      |
|                       | 32 GB **IOPS**: 7k    | **RAM**: 256 - 512 GB |
|                       | (min: 2.5k)           | **IOPS**: 20k (min:   |
|                       |                       | 15k) 2 HA instances:  |
|                       |                       | 1RO, 1RW              |
+-----------------------+-----------------------+-----------------------+
| **Storage** (SSD)     | depends on retention  | 10 TB                 |
|                       | period                |                       |
+-----------------------+-----------------------+-----------------------+
| **AWS** (reference)   | **API Service +       | **API                 |
|                       | Captive               | Servi                 |
|                       | Core**\ \ ``m5.2      | ce**\ \ ``c5.xlarge`` |
|                       | xlarge``\ \ **Databas | (*n*) \ **Captive     |
|                       | e**\ \ ``r5.2xlarge`` | Cor                   |
|                       | (ro)\ ``r5.xlarge``   | e**\ \ ``c5.2xlarge`` |
|                       | (rw)                  | (x2)\ **Database**    |
|                       |                       | ``r5.16xlarge``       |
|                       |                       | (ro)\ ``r5.8xlarge``  |
|                       |                       | (rw)                  |
+-----------------------+-----------------------+-----------------------+

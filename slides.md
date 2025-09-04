---
theme: slidev-theme-presentation
background: https://plus.unsplash.com/premium_photo-1664297989345-f4ff2063b212?q=80&w=1998&auto=format&fit=crop&ixlib=rb-4.1.0&ixid=M3wxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8fA%3D%3D
title: Introduction à la Haute Disponibilité
info: |
  Présentation de la Haute Disponibilité.
  Mise en place dans un contexte de bases de données et de serveurs Web.
class: text-center
drawings:
  persist: false
transition: slide-left
mdc: true
seoMeta:
  ogImage: auto

version: 2.0.1
author: D. Rigaudière
---

<div class="text-center text-4xl font-bold p-3">
Introduction à la Haute Disponibilité
</div>
Cas des serveurs Web et Bases de données

<div class="abs-bl m-6 text-sm text-gray-600 text-">
  v. {{ $slidev.configs.version }} / {{ $slidev.configs.author }}
</div>

<div class="abs-br m-6 text-base">
  <button @click="$slidev.nav.openInEditor()" title="Open in Editor" class="slidev-icon-btn">
    <carbon:edit />
  </button>
  <a href="https://github.com/Weasel-DF/ha-bdd" target="_blank" class="slidev-icon-btn">
    <carbon:logo-github />
  </a>
</div>

<!--
The last comment block of each slide will be treated as slide notes. It will be visible and editable in Presenter Mode along with the slide. [Read more in the docs](https://sli.dev/guide/syntax.html#notes)
-->

---
transition: slide-left
---

# Introduction et concepts clés

## Qu’est-ce que la haute disponibilité ?

- Capacité d’un système à rester opérationnel malgré des pannes
- Objectif : **minimiser les interruptions de service**
- Exemple :
  - Site e-commerce indisponible = perte de chiffre d’affaires
  - Application critique indisponible = impact métier majeur

---
transition: fade-out
---

# Introduction et concepts clés

## Disponibilité et SLA

- **Disponibilité (%)** : proportion de temps où le service est accessible
- **SLA** (Service Level Agreement) : engagement contractuel de disponibilité

| Disponibilité | Temps d’indisponibilité/an |
|---------------|----------------------------|
| 99%           | ~3,65 jours                |
| 99,9%         | ~8 h 45 min                |
| 99,99%        | ~52 min                    |
| 99,999%       | ~5 min                     |

---
transition: fade-out
---

# Introduction et concepts clés

## RPO & RTO

- **RPO (Recovery Point Objective)**

  → Perte de données maximale acceptable après un incident

  → Exemple : « pas plus de 5 min de données perdues »

- **RTO (Recovery Time Objective)**

  → Temps maximal pour rétablir le service

  → Exemple : « service restauré en moins de 30 minutes »

---
transition: fade-out
---

# Introduction et concepts clés

## Causes d’indisponibilité

- Pannes matérielles (serveurs, disques, réseau)
- Pannes logicielles (bugs, corruption de données)
- Erreurs humaines (mauvaise manipulation)
- Catastrophes naturelles ou sinistres (incendie, inondation)

---
transition: fade-out
---

# Introduction et concepts clés

## Théorème CAP

- **C**onsistency (cohérence des données)
- **A**vailability (disponibilité)
- **P**artition tolerance (tolérance aux partitions réseau)

⚠️ Dans un système distribué, on ne peut garantir **que 2 propriétés sur 3**.

Exemples :
- Bases orientées **CA** : priorité à la cohérence et disponibilité locale (SQL traditionnels).
- Bases orientées **AP** : disponibilité et tolérance réseau (Cassandra).
- Bases orientées **CP** : cohérence et tolérance réseau (MongoDB, etc.).

---
transition: fade-out
---

# Introduction et concepts clés

## Théorème CAP

### Les 3 propriétés
- **C** : Cohérence (*Consistency*)  
- **A** : Disponibilité (*Availability*)  
- **P** : Tolérance au partitionnement (*Partition tolerance*)  

### Pourquoi pas les 3 ?
- En cas de **partition réseau** :
  - Garantir **C** → on sacrifie **A** (certaines requêtes refusées)  
  - Garantir **A** → on sacrifie **C** (données différentes selon les nœuds)  
- **P** est incontournable : toute infra distribuée peut subir une partition  

---
transition: fade-out
---

# Introduction et concepts clés

## Conclusion

La haute disponibilité repose sur une approche **globale** :
- Identifier les SPOF
- Mettre en place de la redondance et des mécanismes de reprise
- Tester régulièrement la résilience du système

---
transition: fade-out
---

# Stratégies de haute disponibilité

## Redondance
- Duplication de composants (serveurs, alimentations, liens réseau…)
- Permet de basculer en cas de panne

## Réplication
- Synchronisation de données entre plusieurs systèmes
- Mode synchrone vs asynchrone

## Basculement (Failover)
- Détection de panne et redirection automatique vers un nœud fonctionnel

---
transition: fade-out
---

# Stratégies de haute disponibilité

## Réplication Maître–Esclave

### Principe
- **Un seul maître** : reçoit les écritures (INSERT, UPDATE, DELETE)
- **Un ou plusieurs esclaves** : reçoivent une copie des données en lecture seule
- Réplication souvent **asynchrone**

### ✅ Avantages
- Répartition des **lectures** sur plusieurs serveurs
- Sauvegarde des esclaves sans impacter le maître
- Simplicité de mise en œuvre

### ❌ Limites
- Point de défaillance unique (le maître)
- Basculement manuel souvent nécessaire
- Latence possible entre maître et esclaves

---
transition: fade-out
---

# Stratégies de haute disponibilité

## Réplication Synchrone vs Asynchrone

### Synchrone
- Chaque **transaction** doit être confirmée par le secondaire **avant** d’être validée.  
- ✅ Sécurité : pas de perte de données (RPO ≈ 0)
- ❌ Latence plus élevée

### Asynchrone
- Le primaire confirme immédiatement la transaction au client.
- Les données sont envoyées au secondaire **en différé**.
- ✅ Performances meilleures, faible latence
- ❌ Risque de perte des dernières transactions si le primaire tombe

---
transition: fade-out
---

# Stratégies de haute disponibilité

## Outils techniques

- **Load Balancer** : répartit le trafic, détecte les nœuds en panne
- **Cluster de serveurs** : exécution coordonnée pour tolérance de panne
- **Stockage distribué** : SAN, Ceph, GlusterFS
- **Supervision et alerting** : Prometheus, Zabbix, Grafana

---
transition: fade-out
---

# Stratégies de haute disponibilité

## Load Balancing

- Répartit les requêtes entre plusieurs serveurs pour :
  - **Améliorer les performances** (scalabilité horizontale)
  - **Éviter la surcharge** d’un nœud
  - **Augmenter la disponibilité** (si un serveur tombe, le trafic continue ailleurs)

### Exemples
- HAProxy, Nginx, Traefik
- AWS ELB, Azure Load Balancer

---
transition: fade-out
---

# Stratégies de haute disponibilité

## Quorum

- Principe : **la majorité des nœuds doit être d’accord** pour prendre une décision (élection, bascule, cohérence).
- Évite que plusieurs serveurs prennent des décisions contradictoires.

### Exemple
- Cluster de 5 nœuds : il faut au moins 3 votes pour valider un leader.
- Utilisé dans : **etcd, Consul, Zookeeper, Patroni, Galera**.

---
transition: fade-out
---

# Stratégies de haute disponibilité

## Split-Brain

- Situation où deux nœuds (ou groupes de nœuds) d’un cluster pensent être le **maître** en même temps.
- Conduit à :
  - **Incohérence des données**
  - Risque de **corruption**

### Prévention
- Utilisation du **quorum**
- Mise en place de mécanismes de **fencing** (isoler le nœud défaillant)
- Outils : STONITH (*Shoot The Other Node In The Head*) dans Pacemaker/Corosync

---
transition: fade-out
---

# Stratégies selon les SGBD

## MySQL / MariaDB
- Réplication **asynchrone** maître → esclave
- **Réplication semi-synchrone** (optionnelle, moins de perte de données)
- Outils : Galera Cluster, Percona XtraDB Cluster

---
transition: fade-out
---

# Stratégies selon les SGBD

## PostgreSQL
- Réplication **en continu** (Write-Ahead Log)
- Réplication **synchrone** possible
- **Patroni** + etcd/Consul : gestion automatique du failover
- Solutions avancées : Citus (scalabilité)

---
transition: fade-out
---

# Stratégies selon les SGBD

## SQL Server (Microsoft)
- **AlwaysOn Availability Groups**
- Réplication transactionnelle ou par snapshots
- Solutions matérielles : clustering Windows Server Failover

---
transition: fade-out
---

# Stratégies selon les SGBD

## Oracle Database
- **Real Application Clusters (RAC)** : plusieurs nœuds accèdent simultanément à la même base
- **Data Guard** : réplication synchrone/asynchrone pour PRA/PCA
- Options de **failover automatique** intégrées

---
transition: fade-out
---

# Choisir une stratégie

- Dépend des **contraintes métier** :
  - RTO/RPO exigés
  - Budget disponible
  - Compétences techniques internes
- Souvent : compromis entre **performance**, **coût** et **tolérance aux pannes**

---
transition: slide-left
---

# TP – Mise en place d’une HA MariaDB avec Galera

## Objectifs
- Déployer un **cluster MariaDB Galera** (3 nœuds minimum)
- Configurer la **réplication synchrone** entre les nœuds
- Tester le **failover** et la **tolérance aux pannes**

## Étapes
1. Installer MariaDB et Galera sur chaque VM / conteneur
2. Configurer le fichier `my.cnf` (cluster name, IP des nœuds, SST/IST)
3. Initialiser le **premier nœud** et joindre les suivants
4. Vérifier la réplication (insertion sur un nœud → visible sur tous)
5. Simuler une panne d’un nœud et tester la continuité

[Exemple de procédure](https://www.it-connect.fr/comment-mettre-en-place-mariadb-galera-cluster-sur-debian-11/)
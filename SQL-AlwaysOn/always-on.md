# SQL Server Always On (groupes de disponibilité)

Couvre : SO-AR-14 à SO-AR-18 (grille Système).

## Le principe

Un **groupe de disponibilité** (AG) réplique une ou plusieurs bases d'un
**réplica primaire** (lecture/écriture) vers des **réplicas secondaires**.
Il repose sur un **cluster de basculement Windows (WSFC)** pour le quorum.
Les applications se connectent à l'**écouteur** (*listener*), un nom DNS +
IP virtuelle qui suit toujours le primaire.

Votre infra : groupe **`AG-GRP45`**, réplicas `SRV-SQLGRP45-01` et
`SRV02-SQL`, écouteur `LST-SQLGPR45.antoine.priv:1433`, base `BDD_GRP45`.
Argus s'y connecte par l'écouteur avec le compte `argus_api`.

## SO-AR-14 — Modes de synchronisation

| Mode | Fonctionnement | Conséquence |
|---|---|---|
| **Synchrone** (*synchronous commit*) | le primaire attend que le secondaire ait **durci** la transaction dans son journal avant de valider | aucune perte de données (RPO = 0), latence d'écriture plus élevée ; permet la bascule **automatique** |
| **Asynchrone** (*asynchronous commit*) | le primaire valide sans attendre | performance, mais perte possible ; bascule **manuelle forcée** uniquement ; adapté à un site distant |

États d'une base : `SYNCHRONIZED`, `SYNCHRONIZING`, `NOT SYNCHRONIZING`.

## SO-AR-15 / 16 — Réplicas et configuration

- Prérequis : WSFC, même version de SQL Server, bases en **mode de
  récupération complet** avec une sauvegarde complète, point de terminaison
  de mise en miroir (port **5022**).
- Création : assistant *New Availability Group* ou T-SQL
  (`CREATE AVAILABILITY GROUP`), choix du mode et du basculement par réplica,
  **ensemencement** automatique ou par sauvegarde/restauration, création de
  l'écouteur.
- Ajouter / retirer un réplica : `ALTER AVAILABILITY GROUP … ADD REPLICA`
  / `REMOVE REPLICA`, puis `JOIN` côté secondaire.
- **Secondaires lisibles** (*readable secondary*) : décharger le reporting.

## SO-AR-17 — Surveillance et maintenance

- **Tableau de bord Always On** dans SSMS, DMV
  `sys.dm_hadr_availability_replica_states`, `sys.dm_hadr_database_replica_states`
  (files d'envoi et de rétablissement).
- Supervision Zabbix : votre parc a remonté
  « AG 'AG-GRP45': All replicas unhealthy » (Désastre) et
  « Replica … is disconnected » — c'est cette surveillance qui est évaluée.
- **Sauvegardes sur le secondaire** : préférence de sauvegarde de l'AG
  (*Prefer secondary*), sauvegardes de journal fréquentes, `COPY_ONLY` pour
  les complètes sur un secondaire.
- Automatiser : plans de maintenance / tâches Agent SQL, alertes.

## SO-AR-18 — Basculement

| Type | Quand | Perte |
|---|---|---|
| **Automatique** | primaire tombe, partenaire synchrone, réplica `SYNCHRONIZED` | aucune |
| **Manuel planifié** | maintenance (`ALTER AVAILABILITY GROUP … FAILOVER`) | aucune (synchrone) |
| **Forcé** | secondaire asynchrone, sinistre (`FORCE_FAILOVER_ALLOW_DATA_LOSS`) | possible |

Côté application : grâce à l'écouteur, Argus n'a rien à changer ; ajouter
`MultiSubnetFailover=True` à la chaîne de connexion accélère la reconnexion
si les réplicas sont sur des sous-réseaux différents.

## Questions probables

1. *Synchrone ou asynchrone, que choisissez-vous et pourquoi ?*
2. *À quoi sert l'écouteur ?*
3. *Qu'est-ce que le quorum ?* → la majorité de votes du cluster qui décide
   quel nœud est vivant, pour éviter deux primaires (*split brain*).
4. *Comment testez-vous une bascule ?*

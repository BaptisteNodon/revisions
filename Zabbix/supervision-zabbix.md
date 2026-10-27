# Supervision avec Zabbix 7.0

Couvre : SMSI-001 à SMSI-009 (grille Supervision).

## Le vocabulaire Zabbix (à employer tel quel)

| Terme | Définition |
|---|---|
| **Hôte** | machine ou équipement supervisé |
| **Groupe d'hôtes** | regroupement (droits, filtres) : `Serveurs/Linux`, `Serveurs/Windows`, `Discovered hosts` |
| **Item** | une mesure (`system.cpu.util`, `vm.memory.util`, `vfs.fs.dependent.size[/,pused]`, `icmppingsec`) |
| **Trigger** (déclencheur) | expression sur des items qui produit un **problème** avec une **sévérité** (0 Non classé → 5 Désastre) |
| **Template** (modèle) | ensemble réutilisable d'items, triggers, graphiques, règles de découverte |
| **Action** | ce qu'on fait à l'apparition d'un problème (message, webhook vers Argus, commande) |
| **Media type** | canal d'envoi (e-mail, **webhook**) |
| **Problème** | occurrence d'un trigger ; peut être **acquitté**, commenté, fermé |
| **Historique / tendances** | `history` = valeurs brutes ; `trends` = min/moy/max par **heure** |

## SMSI-001 / 003 — Sources et protocoles

| Source / protocole | Usage | Limites |
|---|---|---|
| **Agent passif** (10050) | le serveur interroge l'agent | le serveur doit joindre chaque hôte |
| **Agent actif** (10051) | l'agent pousse vers le serveur | traverse mieux les pare-feu |
| **SNMP** (161 UDP, traps 162) | équipements réseau, pfSense | MIB, utiliser v3 |
| **IPMI** | matériel (températures, alimentations) | serveurs physiques |
| **ICMP ping** | joignabilité (`icmpping`, `icmppingsec`) | ne dit rien des services |
| **Logs / Event Log** | `log[]`, `eventlog[]` | volume à maîtriser |
| **Scripts / UserParameter** | mesure sur mesure | à maintenir |
| **HTTP agent / web scenario** | tester une API (`/health` d'Argus) | |

## SMSI-002 — Nomenclature et tags

Groupes hiérarchiques (`Serveurs/Windows`), noms d'hôtes normalisés
(`SRV-<rôle>-<n>`, `FW-…`), **tags** clé/valeur (`service:mssql`,
`env:prod`) sur hôtes, items et triggers pour filtrer, router les actions et
construire les tableaux de bord.

## SMSI-004 — Installer et configurer

Serveur + base + frontend (Srv-Zabbix, Srv-ZabbixDB, Srv-ZabbixFront),
agents sur les hôtes, templates liés, interface au vert. Sécurité :
changer le compte `Admin` par défaut, **jeton d'API** sur un compte dédié en
lecture seule pour Argus.

## SMSI-005 — Optimiser checks et rétention

- Intervalles adaptés à la criticité.
- **Items dépendants** : une collecte maître, plusieurs valeurs dérivées.
- **Préprocessing** (JSONPath, regex, *discard unchanged*).
- **Rétention** : historique court (7–30 j), **tendances** longues (365 j).
  Argus lit les tendances (`trend.get`) pour ses agrégats quotidiens.

## SMSI-006 — Haute disponibilité

Mode **HA natif** de Zabbix 7.0 : plusieurs nœuds serveur (un actif, les
autres en attente), base commune redondée, frontend derrière un
répartiteur, **proxies** pour répartir la collecte (« Cluster Zabbix »).

## SMSI-007 — Alerting pertinent

Sévérités justes, **hystérésis** (expression de recouvrement), dépendances
entre triggers (pas 30 alertes si le pare-feu tombe), actions avec
escalade, **webhook vers Argus**. Exemple : Argus compte l'indisponibilité
sur les alertes **High et Disaster** car « Moyen » contenait 310
« AppXSvc is not running » sans impact.

## SMSI-008 — Tableaux de bord

Widgets *Problems*, *Graph*, *Top hosts*, *Host availability*, **Maps**
pour cartographier le périmètre ; un tableau par public. Argus ajoute son
**mode NOC** mobile.

## SMSI-009 — Automatisation

**Découverte réseau** (plage d'IP), **auto-enregistrement** des agents
actifs (`HostMetadata` → action qui ajoute groupe et template),
**LLD** (disques, interfaces, services → items créés automatiquement,
macros `{#FSNAME}`). Le groupe `Discovered hosts` prouve qu'une découverte
tourne.

## Questions probables

1. *Agent passif ou actif, et pourquoi ?*
2. *Différence historique / tendance ?*
3. *Comment éviter une tempête d'alertes ?*
4. *Comment Argus récupère les alertes ?* → `problem.get` toutes les 5 min +
   webhook temps réel.

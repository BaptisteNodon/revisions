# Synthèse des compétences à réviser

Une ligne par critère de `Grilles_evaluation_DIIAGE_v2.xlsx` : ce qu'il faut
savoir dire, et la fiche détaillée. Échelle : Non réalisé 0 · Insuffisant
0,5 · En cours 1,5 · **Acquis 2,5** · **Maîtrisé 4**.

État dans Argus : ✅ couvert · 🔸 partiel · ⬜ à préparer à l'oral (relève
des OPS).

## 1. Mobile (Kotlin)

| Code | Compétence | À savoir | Argus | Fiche |
|---|---|---|---|---|
| CRIT-DMA-D2-S-1 | Rôle de la Clean Architecture | couches, dépendances vers l'intérieur, testabilité | ✅ | [Clean Architecture](Architecture/clean-architecture.md) |
| CRIT-DMA-D2-S-5 | Fondamentaux d'une app | Manifest, dossiers, Gradle, Activity et cycle de vie, Context, Intent | ✅ | [Fondamentaux](Android/fondamentaux-android.md) |
| CRIT-DMA-D2-SE-4 | Expérience dev du projet | Manifest et Gradle sans warning, versions centralisées, extension avec KDoc | 🔸 | [Qualité](Android/qualite-code-kotlin.md) |
| CRIT-DMA-D2-SF-5 | Appliquer les fondamentaux | `libs.versions.toml`, `values-night`, cycle de vie commenté, extension ViewModel/Context/Intent | 🔸 | [Fondamentaux](Android/fondamentaux-android.md) |
| CRIT-DMA-D2-S-2 | Théorie couche Data | Service/DTO, DAO/Entity, mapping, fetch | ✅ / 🔸 pas de Room | [Data et Domain](Android/couches-data-domain.md) |
| CRIT-DMA-D2-S-3 | Théorie couche Domain | Domain Models, interfaces Repository | ✅ | [Data et Domain](Android/couches-data-domain.md) |
| CRIT-DMA-D2-SE-1 | Qualité couche Data | KDoc, découpage, pas de duplication | 🔸 | [Data et Domain](Android/couches-data-domain.md) |
| CRIT-DMA-D2-SE-3 | Qualité couche Domain | rien d'externe dans le Domain, headers métier | ✅ | [Data et Domain](Android/couches-data-domain.md) |
| CRIT-DMA-D2-SF-1 | Intégration Clean Architecture | couplage faible, **Koin** | ✅ | [Clean Architecture](Architecture/clean-architecture.md) |
| CRIT-DMA-D2-SF-3 | Intégration Domain | modèles et interfaces indépendants | ✅ | [Data et Domain](Android/couches-data-domain.md) |
| CRIT-DMA-D2-SF-4 | Intégration Data | fetch **local et distant**, mapping | 🔸 distant seul | [Data et Domain](Android/couches-data-domain.md) |
| CRIT-DMA-D2-S-4 | Théorie UI Compose | déclaratif, état, **MVI + UDF**, Single Activity, navigation | ✅ | [UI Compose](Android/ui-compose-mvi.md) |
| CRIT-DMA-D2-SE-2 | Qualité couche UI | composables en LEGO, State de représentation, KDoc | ✅ | [UI Compose](Android/ui-compose-mvi.md) |
| CRIT-DMA-D2-SF-2 | Intégration UI | une Activity, Screen + ViewModel + UiState | ✅ | [UI Compose](Android/ui-compose-mvi.md) |
| CRIT-DMA-D2-S-6 | Théorie hardware | Context médiateur, Managers, permissions | ✅ | [Capacités natives](Android/capacites-natives.md) |
| CRIT-DMA-D2-SE-5 | Qualité code natif | Managers, Lint, extension Context avec KDoc | 🔸 | [Capacités natives](Android/capacites-natives.md) |
| CRIT-DMA-D2-SF-6 | Pratique hardware | **2 capacités natives**, accès centralisé hors UI | 🔸 biométrie seule | [Capacités natives](Android/capacites-natives.md) |

## 2. API .NET

| Code | Compétence | À savoir | Argus | Fiche |
|---|---|---|---|---|
| API-01 | Modèle de données | normalisation, relations, contraintes, index justifiés | ✅ | [Modèle et REST](API-NET/modele-donnees-et-rest.md) |
| API-02 | Endpoints REST | verbes, codes HTTP, pagination curseur | ✅ | [Modèle et REST](API-NET/modele-donnees-et-rest.md) |
| API-03 | Authentification & sécurité | bind AD, JWT, refresh token haché, rôles | ✅ (AD prod en cours) | [Sécurité](API-NET/securite-authentification.md) |
| API-04 | Service externe | client Zabbix, miroir, rejeu, cache, webhook | ✅ | [Intégration](API-NET/integration-qualite-documentation.md) |
| API-05 | Qualité & tests | 4 couches ; pas de tests auto côté back | 🔸 | [Intégration](API-NET/integration-qualite-documentation.md) |
| API-06 | Documentation | contrat OpenAPI partagé, mock, `check-contract.py` | ✅ | [Intégration](API-NET/integration-qualite-documentation.md) |

## 3. Système (Windows & SQL)

| Code | Compétence | À savoir | Argus | Fiche |
|---|---|---|---|---|
| SO-AR-1 | Fonctionnement LDAP | DN/OU/CN/DC, bind, search, 389/636, **avec un schéma** | ✅ | [LDAP / AD](Annuaire-LDAP/ldap-active-directory.md) |
| SO-AR-2 | Installer un annuaire | AD DS, promotion, rôles FSMO | ⬜ | [LDAP / AD](Annuaire-LDAP/ldap-active-directory.md) |
| SO-AR-3 | Objets de l'annuaire | utilisateurs, ordinateurs, groupes, OU | ⬜ | [LDAP / AD](Annuaire-LDAP/ldap-active-directory.md) |
| SO-AR-4 | Structurer l'arborescence | OU logiques, AGDLP | ⬜ | [LDAP / AD](Annuaire-LDAP/ldap-active-directory.md) |
| SO-AR-5 | Déployer des configurations | GPO, ordre LSDOU, filtrage | ⬜ | [LDAP / AD](Annuaire-LDAP/ldap-active-directory.md) |
| SO-AR-6 | Bonnes pratiques annuaire | deux DC, mises à jour, comptes dédiés | ⬜ | [LDAP / AD](Annuaire-LDAP/ldap-active-directory.md) |
| SO-AR-7 | Sécuriser un annuaire | mots de passe, délégation, bastion | ⬜ | [LDAP / AD](Annuaire-LDAP/ldap-active-directory.md) |
| SO-AR-8 | Délégation de droits | délégation sur une OU, comptes d'admin dédiés | ⬜ | [LDAP / AD](Annuaire-LDAP/ldap-active-directory.md) |
| SO-AR-9 | DC en lecture seule | RODC, PRP (mise en cache) | ⬜ | [LDAP / AD](Annuaire-LDAP/ldap-active-directory.md) |
| SO-AR-10 | Règles de sécurité LDAP | signature, LDAPS, tiering | ✅ côté appli | [LDAP / AD](Annuaire-LDAP/ldap-active-directory.md) |
| SO-AR-12 | Authentification sécurisée | via LDAP sécurisé = LDAPS (ce que fait Argus) | ✅ | [LDAP / AD](Annuaire-LDAP/ldap-active-directory.md) |
| SO-AR-11 | Configurer un helpdesk | GLPI : profils, catégories, règles, SLA | ⬜ | [Helpdesk](Helpdesk/helpdesk.md) |
| SO-AR-13 | Bonnes pratiques helpdesk | auth AD, HTTPS, SLA, sauvegarde | ⬜ | [Helpdesk](Helpdesk/helpdesk.md) |
| SO-AR-14 | Modes de synchronisation | synchrone / asynchrone, RPO | ⬜ | [Always On](SQL-AlwaysOn/always-on.md) |
| SO-AR-15 | Gérer les réplicas | ajouter, retirer, joindre, secondaire lisible | ⬜ | [Always On](SQL-AlwaysOn/always-on.md) |
| SO-AR-16 | Configurer un AG | WSFC, mode complet, port 5022, écouteur | 🔸 AG-GRP45 | [Always On](SQL-AlwaysOn/always-on.md) |
| SO-AR-17 | Surveillance et maintenance | tableau de bord, DMV, sauvegardes secondaires | 🔸 | [Always On](SQL-AlwaysOn/always-on.md) |
| SO-AR-18 | Basculement | automatique, manuel, forcé, quorum | ⬜ | [Always On](SQL-AlwaysOn/always-on.md) |

## 4. Supervision

| Code | Compétence | À savoir | Argus | Fiche |
|---|---|---|---|---|
| SMSI-001 | Trouver l'information | logs, Event Log, SNMP, IPMI, scripts | ✅ | [Zabbix](Zabbix/supervision-zabbix.md) |
| SMSI-002 | Nomenclature et tags | groupes, noms normalisés, tags | ✅ | [Zabbix](Zabbix/supervision-zabbix.md) |
| SMSI-003 | Choisir l'outil / le protocole | agent passif/actif, SNMP, ICMP | ✅ | [Zabbix](Zabbix/supervision-zabbix.md) |
| SMSI-004 | Installer et configurer | serveur, base, frontend, agents, templates | ✅ | [Zabbix](Zabbix/supervision-zabbix.md) |
| SMSI-005 | Checks et rétention | items dépendants, préprocessing, historique vs tendances | ✅ | [Zabbix](Zabbix/supervision-zabbix.md) |
| SMSI-006 | Haute disponibilité | HA natif Zabbix 7.0, proxies | 🔸 | [Zabbix](Zabbix/supervision-zabbix.md) |
| SMSI-007 | Alerting pertinent | sévérités, hystérésis, dépendances, webhook | ✅ | [Zabbix](Zabbix/supervision-zabbix.md) |
| SMSI-008 | Tableaux de bord | widgets, Maps ; mode NOC d'Argus | ✅ | [Zabbix](Zabbix/supervision-zabbix.md) |
| SMSI-009 | Automatisation | découverte réseau, auto-enregistrement, LLD | ✅ | [Zabbix](Zabbix/supervision-zabbix.md) |

## 5. Infrastructure

| Code | Compétence | À savoir | Argus | Fiche |
|---|---|---|---|---|
| SO-CY-1 | Fonctionnement d'un pare-feu | pfSense, stateful, ordre des règles, NAT, alias | ⬜ | [pfSense / DMZ](Reseau-Securite/pfsense-dmz.md) |
| SO-CY-2 | Mettre en place un pare-feu | refus par défaut, règles précises, admin non exposée | ⬜ | [pfSense / DMZ](Reseau-Securite/pfsense-dmz.md) |
| SO-CY-4 | DMZ | zone tampon, filtrage DMZ → LAN au port près | ⬜ | [pfSense / DMZ](Reseau-Securite/pfsense-dmz.md) |
| SO-CY-5 | Bonnes pratiques pare-feu | moindre ouverture, VPN limité, journalisation | ⬜ | [pfSense / DMZ](Reseau-Securite/pfsense-dmz.md) |
| ASI-4 | Documenter l'architecture de prod | rôles des serveurs, flux, protocoles, topologie | 🔸 | [Architecture d'Argus](Architecture/architecture-argus.md) |
| ASI-5 | Architecture logique et physique | types (n-tiers…), justifier celle du projet | ✅ 3-tiers | [Architecture d'Argus](Architecture/architecture-argus.md) |
| ASI-6 | Documenter l'architecture logique | schéma complet + explication orale | 🔸 | [Architecture d'Argus](Architecture/architecture-argus.md) |
| ASI-7 | Documenter la sécurité | règles de filtrage justifiées | 🔸 | [pfSense / DMZ](Reseau-Securite/pfsense-dmz.md) |
| OSI-700-1 | Hyperviseur de type 1 | bare-metal vs hébergé, ESXi / Proxmox / Hyper-V | ⬜ | [Virtualisation](Virtualisation/virtualisation.md) |
| OSI-700-2 | Clonage | complet vs lié, Sysprep (SID), modèle | ⬜ | [Virtualisation](Virtualisation/virtualisation.md) |
| OSI-700-3 | Gestion des ressources | vCPU, RAM, disques, déployer depuis un modèle | ⬜ | [Virtualisation](Virtualisation/virtualisation.md) |
| OSI-700-4 | Réseaux virtuels | vSwitch, groupes de ports / VLAN, uplink | ⬜ | [Virtualisation](Virtualisation/virtualisation.md) |
| OSI-700-5 | Documenter son projet | une équipe reprend sans autre information | 🔸 | [Architecture d'Argus](Architecture/architecture-argus.md) |
| SO-OSI-700-C21 | Installer un hyperviseur | installation propre, optimisée, documentée | ⬜ | [Virtualisation](Virtualisation/virtualisation.md) |
| SO-OSI-700-C22 | Configuration réseau | nœud accessible, VM qui communiquent | ⬜ | [Virtualisation](Virtualisation/virtualisation.md) |
| SO-OSI-700-C23 | Créer des VM | options optimisées pour la prod | ⬜ | [Virtualisation](Virtualisation/virtualisation.md) |
| SO-OSI-700-C24 | Stockage | disques de VM, ISO, modèles, sauvegardes | ⬜ | [Virtualisation](Virtualisation/virtualisation.md) |
| SO-OSI-700-C25 | Sécuriser la plateforme | comptes, réseau d'admin, sauvegardes + test de restauration | ⬜ | [Virtualisation](Virtualisation/virtualisation.md) |

## 6. CI/CD (GitLab)

| Code | Compétence | À savoir | Argus | Fiche |
|---|---|---|---|---|
| DSO-DA-200-1 | Fonctionnement de GitLab | groupe, projets, MR, jalons, runners | ✅ | [GitLab CI/CD](CI-CD/gitlab-ci-cd.md) |
| DSO-DA-200-2 | Travail en équipe | branches → `dev` → `master` (livraison sur master) | 🔸 front pas livré | [GitLab CI/CD](CI-CD/gitlab-ci-cd.md) |
| DSO-DA-200-3 | CI/CD sous GitLab | chaîne sur la branche de dev, livrer l'APK | 🔸 pas de runner | [GitLab CI/CD](CI-CD/gitlab-ci-cd.md) |
| DFS-04 | Intégration et livraison continues | stratégie documentée + jobs | 🔸 | [GitLab CI/CD](CI-CD/gitlab-ci-cd.md) |
| DSO-DA-200-5 | Définir sa chaîne | pipeline, stage, job, artefact, cache, rules | ✅ | [GitLab CI/CD](CI-CD/gitlab-ci-cd.md) |
| DSO-DA-200-7 | Documenter la chaîne | étapes, déclencheurs, secrets, justifiés | 🔸 | [GitLab CI/CD](CI-CD/gitlab-ci-cd.md) |
| C-DSO-SS-5 | Collaboration | contrat d'API, MR relues, échanges OPS | ✅ | [GitLab CI/CD](CI-CD/gitlab-ci-cd.md) |
| DI3-DSO-DDO-C3-CRIT1 | Configuration et secrets | variables d'environnement, `.env` gitignoré, variables CI masquées | ✅ | [GitLab CI/CD](CI-CD/gitlab-ci-cd.md) |

## Ce qui rapporterait le plus de points avant la soutenance

1. **Une fonction d'extension sur `Context` avec KDoc complet** : elle
   compte pour trois critères Mobile (SE-4, SF-5, SE-5/SF-6).
2. **Une seconde capacité native** (vibration sur alerte critique, ou Room).
3. **Fusionner `dev` dans `master` côté front** (DSO-DA-200-2).
4. **Un runner GitLab actif et un job de tests** (DSO-DA-200-3, DFS-04).
5. **Zéro warning Manifest et Gradle** (`./gradlew lint`).

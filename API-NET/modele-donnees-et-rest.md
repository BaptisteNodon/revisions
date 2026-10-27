# Modèle de données et endpoints REST

Couvre : API-01, API-02.

## API-01 — Modèle de données

- **Normalisation** : chaque information à un seul endroit.
  1FN valeurs atomiques · 2FN pas de dépendance à une partie de la clé ·
  3FN pas de dépendance entre colonnes non-clés.
- **Relations** : 1-N (`hosts` → `alerts` via `alerts.host_id`), N-N par table
  d'association (`host_group`, `user_role`), 1-1 (`user_preferences` dont la
  clé primaire est aussi la clé étrangère vers `users`).
- **Intégrité** : clés primaires, étrangères, `UNIQUE`, `CHECK`.

Dans Argus, le schéma est **écrit à la main**
(`backend/database/argus_schema_final.sql`) ; c'est lui qui fait foi, pas
les migrations EF. Points à mettre en avant pour « optimisé, indexé et
justifié » :

| Choix | Justification |
|---|---|
| `zabbix_id` UNIQUE sur `hosts`, `zabbix_event_id` UNIQUE sur `alerts` | rend la synchronisation **idempotente** : rejouer ne crée pas de doublon |
| Suppression logique (`deleted`) | on garde l'historique d'incidents d'un hôte retiré |
| `daily_availabilities`, `daily_metrics` (agrégats journaliers) | un rapport d'un mois = quelques centaines de lignes, pas des millions de points |
| `daily_metrics` en **clé-valeur** (`metric_code`) | ajouter une métrique ne demande aucune migration |
| Index `ix_daily_availabilities_period (day, host_id)`, `ix_sync_runs_latest` | servent les requêtes de rapport et de statut |
| Refresh tokens stockés en **SHA-256** (`VARBINARY(32)`) | une fuite de la base ne donne aucun jeton utilisable |
| Contraintes `CHECK` (`ck_sync_runs_job_type`…) | la base refuse les valeurs invalides |

## API-02 — Endpoints REST

- **Ressources au pluriel**, verbes HTTP pour l'action :
  `GET /api/hosts`, `GET /api/hosts/{id}`, `PUT /api/hosts/{id}`,
  `POST /api/alerts/{id}/acknowledge`.
- **Codes HTTP** : 200 OK, 201 Created, 204 No Content (PUT réussi),
  400 requête invalide (`ProblemDetails`), 401 non authentifié,
  403 rôle insuffisant, 404 introuvable, 409 conflit (alerte déjà acquittée).
- **Pagination par curseur** (keyset) : `?cursor=…&limit=…` → `nextCursor`.
  Plus stable que `offset` quand des lignes arrivent pendant la lecture.
- **Filtres en query string** : `/api/alerts?status=Active&minSeverity=High`.
- **Énumérations en chaînes** (`"Disaster"`, pas `5`) : lisible et stable.
- **Routes écrites en dur** (`[Route("api/hosts")]`), pas `[controller]` :
  renommer une classe ne casse pas le contrat.
- **Idempotence** : GET, PUT, DELETE le sont ; POST ne l'est pas. L'ingestion
  du webhook est rendue idempotente par `zabbix_event_id`.
- **Évolution** : pour « Maîtrisé », parler de versionnement (`/api/v1`) —
  Argus ne l'a pas, mais le contrat figé joue ce rôle de stabilité.

## Questions probables

1. *Pourquoi 204 et pas 200 sur le PUT ?* → rien à renvoyer.
2. *Différence 401 / 403 ?*
3. *Pourquoi une pagination par curseur ?*
4. *Montrez une table N-N.* → `host_group`.
5. *Pourquoi des agrégats journaliers ?*

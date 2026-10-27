# Service externe, qualité et documentation de l'API

Couvre : API-04, API-05, API-06.

## API-04 — Intégration d'un service externe (Zabbix)

Argus consomme l'**API JSON-RPC de Zabbix 7.0** et l'expose en REST.

| Mécanisme | Dans Argus |
|---|---|
| Client unique isolé derrière une interface | `IZabbixClient` / `ZabbixClient` : le reste du code ignore le JSON-RPC |
| **Miroir local** | les hôtes et alertes sont recopiés en base : le mobile répond même si Zabbix est en panne |
| Synchronisation périodique | `ZabbixSyncJob` (parc), `AlertSyncJob` (alertes), toutes les 5 min |
| Temps réel | webhook Zabbix → `/api/zabbix/webhook`, idempotent sur `zabbixEventId` |
| **Résilience** | rejeu des lectures avec attente exponentielle (250 ms × 2ⁿ) ; **jamais** de rejeu d'un acquittement (non idempotent) |
| File de rejeu | une action non transmise reste `Pending` et `PendingActionRetryJob` la renvoie |
| **Cache** | cache de 5 min sur le réglage « fermeture manuelle » d'un déclencheur |
| Garde-fou | une liste d'hôtes vide est refusée plutôt que de vider le parc |
| Journal | `sync_runs` trace chaque passage (succès, échec, nombre d'éléments) |

Pièges Zabbix 7.0 à citer : réponse **toujours en HTTP 200** (l'erreur est
dans le corps), en-tête `Authorization: Bearer` (le paramètre `auth` a
disparu), entiers en chaînes, disponibilité portée par l'**interface** et
non par l'hôte, `problem.get` refuse `selectHosts`.

## API-05 — Qualité et tests

- **Structure** : 4 couches, dépendances strictes, DI centralisée
  (`ServiceCollectionExtensions`), `Program.cs` réduit à un sommaire.
- **Honnêteté** : le back n'a **pas de projet de tests automatisés**. La
  vérification se fait en exécutant la pile (curl, cas 200/400/401/403/404),
  et par `check-contract.py`, qui compare l'API au contrat. Côté mobile, des
  **tests unitaires** existent (24 tests JUnit sur le filtrage, le tri et
  l'historique).
- Pour viser plus haut : xUnit + projet de test sur `AvailabilityCalculator`
  (fonctions pures déjà isolées : `ComputeDay`, `AggregateTrends`).

## API-06 — Documentation de l'API

- **Contrat OpenAPI figé** : `frontend/docs/api/openapi.yaml`, source de
  vérité partagée entre mobile et back (US-01). Exemples au niveau de chaque
  schéma, `x-status` (`implemented` / `planned`), `x-user-story`.
- **Mock** (Prism, port 4010) : le mobile développe sans backend.
- **Vérifications automatiques** : `check-mock.py --dry-run` (le contrat se
  suffit à lui-même), `check-contract.py` (l'API respecte le contrat).
- **Swagger / Scalar** générés par l'API en Development.

C'est exactement le niveau « Maîtrisé » de API-06 : *documentation complète,
exemples, contrat d'API partagé*. À régler avant la soutenance : 15 écarts
connus (`/api/park`, `/api/sync/status`, webhook).

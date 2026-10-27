# Système de gestion d'incidents (helpdesk)

Couvre : SO-AR-11, SO-AR-13 (grille Système).

> Ces critères ne correspondent à aucune partie d'Argus côté DEV : ils
> relèvent de la filière OPS. À vérifier avec eux : quel outil a été
> installé (GLPI est le plus courant en formation).

## Les notions (référentiel ITIL)

| Terme | Définition |
|---|---|
| **Incident** | interruption ou dégradation d'un service (« le serveur SQL ne répond plus ») |
| **Demande** | besoin standard (« créer un compte », « installer un logiciel ») |
| **Problème** | cause racine d'un ou plusieurs incidents |
| **Changement** | modification planifiée de l'infrastructure |
| **SLA** | engagement de délai (prise en charge, résolution) |
| **Priorité** | impact × urgence |

## SO-AR-11 — Configuration (exemple GLPI)

Installation (serveur web + PHP + base), puis : entités, catégories,
**profils** (self-service, technicien, admin), groupes de techniciens,
**règles d'attribution** automatique, **collecteur de mails**,
notifications, SLA. « Maîtrisé » : on peut **déclarer et résoudre** un
incident et une demande de bout en bout.

## SO-AR-13 — Bonnes pratiques

1. **Authentification via l'annuaire** (LDAP/AD), droits par groupes AD.
2. **HTTPS** et mises à jour de l'outil.
3. **Catégories et SLA** définis, base de connaissances, profils au moindre
   privilège, sauvegarde de la base.
4. Bonus : **liaison avec la supervision** (Zabbix → ticket automatique sur
   un problème Désastre). Argus joue un rôle voisin côté mobile en traçant
   l'acquittement par compte AD.

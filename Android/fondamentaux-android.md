# Fondamentaux d'une application Android

Couvre : CRIT-DMA-D2-S-5, SF-5, SE-4.

## Structure d'un projet

| Élément | Rôle | Dans Argus |
|---|---|---|
| `AndroidManifest.xml` | carte d'identité de l'app : composants (Activity, Service…), permissions, application, config réseau | 1 seule Activity (`MainActivity`), permissions `INTERNET`, `ACCESS_NETWORK_STATE`, `USE_BIOMETRIC`, `networkSecurityConfig` |
| `build.gradle.kts` (app) | compileSdk/minSdk, buildTypes (debug/release), `buildConfigField`, signature, dépendances | URL de l'API injectée par `buildConfigField` selon debug/release |
| `gradle/libs.versions.toml` | **catalogue de versions** : toutes les versions de dépendances au même endroit | ✅ versions centralisées |
| `res/` | ressources : `values/strings.xml`, `values-night/` (thème sombre), `drawable/`, `mipmap/`, `xml/` | variante `values-night` = une **variante de ressource** (critère SF-5) |
| `src/debug/` | ressources propres au build debug | `network_security_config.xml` qui autorise le HTTP en clair vers 10.0.2.2 |

## Les composants de base

- **Activity** : un écran point d'entrée, avec un **cycle de vie**
  `onCreate → onStart → onResume → (onPause → onStop) → onDestroy`. Une
  rotation détruit et recrée l'Activity — d'où le ViewModel, qui survit.
- **Context** : l'accès au système (ressources, services système, fichiers,
  préférences). `Application` context vit tout le temps de l'app, `Activity`
  context vit le temps de l'écran : ne jamais garder un Activity context dans
  un singleton (fuite mémoire).
- **Intent** : message pour démarrer un composant (explicite : une classe
  précise ; implicite : une action, ex. ouvrir un lien). Sert aussi aux
  notifications (PendingIntent) et au deep link d'une alerte.
- **Cycle de vie côté Compose** : `LaunchedEffect`, `DisposableEffect`,
  `collectAsStateWithLifecycle` (arrête la collecte quand l'écran passe en
  arrière-plan). Dans Argus : `RepeatWhileStartedEffect` relance le
  chargement toutes les 30 s **seulement quand l'écran est visible** — c'est
  une utilisation *pertinente* du cycle de vie (critère SF-5, à commenter).

## Ce que la grille veut voir pour « Maîtrisé » (SF-5 / SE-4)

- [ ] Manifest **sans warning**, commenté.
- [ ] Gradle **sans warning**, sections commentées.
- [x] Versions centralisées (`libs.versions.toml`), nommage pertinent.
- [x] Ressources systématiques + au moins une variante (`values-night`).
- [x] Cycle de vie utilisé à bon escient **et signalé par un commentaire**.
- [ ] **Fonction d'extension sur ViewModel, Context ou Intent**, avec un
      header **KDoc** (`@param`, `@return`, `@receiver`). Argus a des
      extensions (`Instant.toRelativeTime`, `NavHostController.navigateToTab`)
      mais **aucune sur Context/ViewModel/Intent** : c'est le point à ajouter
      ou à savoir expliquer.

Exemple KDoc attendu :

```kotlin
/**
 * Indique si l'appareil peut authentifier l'utilisateur par biométrie.
 *
 * @receiver le Context, qui donne accès au BiometricManager du système.
 * @return true si une empreinte ou un visage est enrôlé et utilisable.
 */
fun Context.canUseBiometrics(): Boolean = …
```

## Questions probables

1. *Que contient le Manifest ?* → composants, permissions, thème, config réseau.
2. *Différence Application context / Activity context ?*
3. *Pourquoi une seule Activity ?* → navigation Compose, état partagé, moins
   de cycles de vie à gérer.
4. *Comment l'URL change entre debug et release ?* → `buildConfigField` par
   buildType.

Voir aussi : [Qualité de code](qualite-code-kotlin.md).

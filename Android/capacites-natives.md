# Capacités natives du téléphone

Couvre : CRIT-DMA-D2-S-6, SE-5, SF-6 (« gestion des données avec la base
native du device »).

## Les notions

- **Capacités natives** : capteurs (gyroscope, accéléromètre), GPS, caméra,
  son/vibration, biométrie, **base de données locale (Room/SQLite)**,
  stockage, notifications.
- **Le Context est le médiateur** : on obtient un service système par lui
  (`context.getSystemService(...)`, `BiometricManager.from(context)`,
  `LocationServices.getFusedLocationProviderClient(context)`).
- **Manager** : une classe dédiée qui *encapsule* l'accès au matériel. L'UI
  appelle `biometricManager.authenticate()` sans rien savoir de l'API
  Android sous-jacente. → UI simple, logique testable, remplaçable.
- **Permissions** : déclarées dans le Manifest ; les **permissions
  dangereuses** (localisation, caméra, micro) se demandent **à l'exécution**
  (`rememberLauncherForActivityResult(RequestPermission())`). Ne demander que
  ce qui sert, au moment où ça sert.

## Dans Argus

| Capacité | État | Où |
|---|---|---|
| Réseau | ✅ | `INTERNET`, `ACCESS_NETWORK_STATE` |
| Biométrie | 🔸 | `ui/core/util/BiometricHelper.kt` (object) — US-10 non branchée |
| Stockage chiffré | ✅ | `TokenStorage` (jetons de session) |
| Base locale Room | ⬜ | entités et DAO en `TODO` (US-23) |
| Notifications push | ⬜ | FCM non branché (US-32) |

**Points faibles face à la grille** :
- `BiometricHelper` est dans `ui/core/util` et prend un Context en
  paramètre : pour être un « Manager » au sens de la grille, il faudrait une
  classe `BiometricManager` côté data/device, injectée par Koin.
- Il faut **au moins deux capacités natives** pour « Maîtrisé » (biométrie +
  Room, ou + vibration à l'arrivée d'une alerte critique).
- Il faut une **fonction d'extension sur Context** avec un header KDoc.

## Exemple à savoir écrire

```kotlin
/**
 * Fait vibrer le téléphone pour signaler une alerte critique.
 *
 * @receiver le Context, qui fournit le service système de vibration.
 * @param durationMs durée de la vibration, en millisecondes.
 */
fun Context.vibrateForAlert(durationMs: Long = 400) {
    val vibrator = getSystemService(Vibrator::class.java) ?: return
    vibrator.vibrate(VibrationEffect.createOneShot(durationMs, VibrationEffect.DEFAULT_AMPLITUDE))
}
```

## Questions probables

1. *Pourquoi passer par un Manager ?* → séparation UI / matériel, test.
2. *Différence permission normale / dangereuse ?*
3. *Pourquoi le Context est-il nécessaire ?* → c'est lui qui donne accès aux
   services système de l'OS.
4. *Comment éviter une fuite mémoire avec le Context ?* → applicationContext
   dans les singletons.

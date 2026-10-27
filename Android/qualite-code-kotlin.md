# Qualité de code Kotlin et « expérience développeur »

Couvre : les critères **SE** de la grille Mobile (SE-1 à SE-5).

## Ce que la grille attend partout

- **Nommage** : conventions Kotlin (classes en `PascalCase`, fonctions et
  variables en `camelCase`, constantes en `UPPER_SNAKE_CASE`), noms
  explicites. Règle Argus : identifiants en anglais, commentaires en
  français.
- **Découpage** : petites fonctions réutilisables, pas de duplication.
  Composables en LEGO.
- **Commentaires** : disent *pourquoi*, pas *quoi*.
- **KDoc** sur les composants complexes et les fonctions d'extension, avec
  des *block tags* pertinents :

| Tag | Sert à |
|---|---|
| `@param nom` | décrire un paramètre |
| `@return` | décrire la valeur rendue |
| `@receiver` | décrire l'objet étendu (extension) |
| `@throws` | exception possible |
| `@see` | renvoi vers un autre élément |
| `@sample` | exemple d'utilisation |

- **Zéro warning** dans le Manifest et Gradle (vérifier avec
  `./gradlew lint` et l'onglet *Problems* d'Android Studio).

## Bons exemples à montrer dans Argus

- `domain/repository/HostRepository.kt` : KDoc qui explique l'intention.
- `ui/core/component/host/SmallGraph.kt` : header qui justifie les courbes
  de Bézier.
- `ui/screens/hosts/list/HostSections.kt` : logique pure, testée
  (`HostSectionsTest`, 11 tests).
- `di/*Module.kt` : commentaires qui expliquent `viewModel` vs `single`.

## Avant la soutenance

1. Lancer `./gradlew lint` et corriger les warnings Manifest/Gradle.
2. Ajouter une extension sur `Context` ou `ViewModel` avec KDoc complet.
3. Relire les headers KDoc : ajouter `@param` / `@return` là où il n'y a
   que du texte.

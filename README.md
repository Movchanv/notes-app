# notes-app

## 🚀 Release Process

This project follows a strict versioning and release workflow to ensure traceability and reproducibility.

### 🔁 Continuous Integration (on push to main)

A chaque push sur `master` (branche principale du projet), le pipeline CI execute automatiquement les etapes suivantes :

- Installation des dependances Node.js (`npm ci`)
- Execution des tests automatises (`npm test`)
- Build de l'image Docker de l'API
- Publication de deux tags d'image sur Docker Hub :
  - `latest` : pointeur vers la derniere build valide de la branche principale
  - `${{ github.sha }}` : tag immuable correspondant au commit exact

Ce pipeline garantit que chaque changement fusionne dans `master` est teste et immediatement publiable.

### 🏷 Creating a Release (Versioned Product)

Une release est declenchee uniquement lors d'un push de tag Git au format `v*` (ex. `v1.0.2`).

Workflow de release :

- Creation d'un tag Git versionne (`git tag vX.Y.Z`)
- Push du tag (`git push origin vX.Y.Z`)
- Declenchement du workflow `release`
- Generation des metadonnees Docker a partir du tag Git
- Build et push de l'image Docker versionnee sur Docker Hub

Le tag Docker produit correspond a la version Git, ce qui permet de relier une version applicative a un etat precis du code.

### 📌 Versioning Rules

Le projet applique un versioning de type SemVer : `vMAJOR.MINOR.PATCH`.

- `MAJOR` : changement non retrocompatible
- `MINOR` : ajout de fonctionnalite retrocompatible
- `PATCH` : correction de bug sans rupture

Regles importantes :

- Une version `vX.Y.Z` est immuable : elle ne doit jamais etre reconstruite ni reassignee
- Le tag `latest` n'est pas une version ; c'est un alias mouvant
- Pour un deploiement de production, privilegier une version explicite (`vX.Y.Z`) ou un digest

### 🔎 Traceability

La tracabilite est assuree par la combinaison de plusieurs identifiants :

- Commit Git (source exacte du code)
- Tag Git versionne (`vX.Y.Z`) pour les releases
- Tag Docker SHA (`${{ github.sha }}`) pour lier image <-> commit
- Historique GitHub Actions (preuves d'execution CI/CD)
- Tags Docker Hub (artefacts publies)

Ainsi, il est possible de remonter de maniere fiable : d'une image deployee -> vers son commit Git -> vers son pipeline d'execution -> vers sa version fonctionnelle.

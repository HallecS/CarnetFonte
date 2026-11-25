# Carnet de Fonte

Tableau de suivi des charges en musculation, pour soi et ses amis.

- Un tableau avec les exercices en lignes et une colonne par personne : charge (kg), séries × répétitions.
- Une flèche ↑ pour se rappeler de tenter plus lourd à la séance suivante.
- Un historique daté de chaque changement, avec la date de la dernière progression et une courbe par exercice.
- Une liste commune de 26 exercices décrits (muscles travaillés, consignes), et la possibilité d'en ajouter.
- Pensé pour le téléphone, en thème clair ou sombre.

## Utilisation

Ouvre la page GitHub Pages et connecte-toi avec Google. Les données sont stockées dans Firebase (Firestore) : tout le monde voit le tableau, chacun ne peut écrire que dans sa propre colonne (imposé par les règles Firestore).

La même page fonctionne aussi comme artefact claude.ai : elle utilise alors la base de l'artefact au lieu de Firebase. Si Firebase est injoignable, elle bascule en mode local (données dans le navigateur de l'appareil).

## Règles Firestore

```
rules_version = '2';
service cloud.firestore {
  match /databases/{database}/documents {
    match /lifters/{uid} {
      allow read: if request.auth != null;
      allow write: if request.auth != null && request.auth.uid == uid;
    }
    match /exercises/{id} {
      allow read: if request.auth != null;
      allow create: if request.auth != null && request.resource.data.by == request.auth.uid;
      allow delete: if request.auth != null && resource.data.by == request.auth.uid;
    }
  }
}
```

## Publier sur GitHub Pages

Settings → Pages → Source : *Deploy from a branch* → branche `main`, dossier `/ (root)`. Ajouter le domaine `<utilisateur>.github.io` dans Firebase → Authentication → Domaines autorisés.

# TP7 — Sécurité des actions tierces

## Relevé de actions/setup-node

Relevé du 19 novembre 2026 :

- Dépôt : https://github.com/actions/setup-node
- Créateur : organisation actions.
- Badge : Verified sur la fiche GitHub Marketplace.
- Nombre d'étoiles affiché : environ 5,0 k, valeur arrondie de l'interface.
- Dossier contenant le code source : src/.
- Le dossier dist/ contient le code compilé distribué avec l'action.

Fiche Marketplace :
https://github.com/marketplace/actions/setup-node-js-environment

Le nombre d'étoiles renseigne sur la popularité du projet, mais ne
constitue pas une garantie de sécurité.

## Q3. Pourquoi utiliser @v4 plutôt que @main ?

@main suit directement l'évolution d'une branche de développement.
Son contenu peut changer entre deux exécutions et introduire des
modifications qui n'ont pas été choisies pour le projet.

@v4 référence une version majeure identifiée et limite les changements
inattendus liés à une nouvelle version majeure.

Cependant, un tag peut être déplacé : @v4 n'est pas une référence
immuable. Pour fixer précisément une révision en production, on peut
utiliser un SHA de commit complet préalablement vérifié.

## Q4. Le badge de créateur vérifié garantit-il qu'une action est sûre ?

Non. Le badge indique que GitHub a vérifié l'identité du créateur
comme organisation partenaire.

Il ne garantit ni l'absence de vulnérabilités, ni la sécurité de
toutes les dépendances, ni l'absence de compromission future.
Le code et la version utilisés doivent aussi être examinés.

## Q5. Deux vérifications avant d'utiliser une action inconnue en production

1. Examiner le code et les dépendances de la version choisie, notamment
   les commandes exécutées, les permissions nécessaires, les accès aux
   secrets et les éventuels envois de données vers des services externes.

2. Vérifier la provenance et la maintenance du projet : identité du
   fournisseur, historique des versions, activité du dépôt, problèmes
   signalés et éventuels avis de sécurité.

## Manipulation des références

Le workflow a été modifié temporairement pour utiliser
actions/checkout@main, puis rétabli sur actions/checkout@v4.

Les deux modifications sont conservées dans l'historique des commits.
La version finale utilise @v4 et limite les permissions du GITHUB_TOKEN
à contents: read.

## Documentation

https://docs.github.com/en/actions/reference/security/secure-use

# TP7 — Actions tierces et actions personnalisées

## Q1. Pourquoi checkout peut-il être utilisé sans with ?

actions/checkout possède des paramètres par défaut adaptés au cas courant :
il récupère le dépôt et la référence associés à l'événement du workflow.

Dans ce TP, setup-node utilise with pour demander explicitement Node.js 20.
Sans cette configuration, la version demandée par l'exercice ne serait
pas garantie.

with n'est donc pas toujours obligatoire pour setup-node. Il sert ici
à choisir précisément la version de Node.js.

## Q2. Pourquoi verify ne retrouve-t-il pas report.txt sans l'artefact ?

Les jobs utilisent des environnements de travail séparés. Le fichier
report.txt est créé pendant l'exécution de build : il n'est pas
automatiquement présent dans l'espace de travail de verify.

Le fait d'utiliser le même dépôt ne signifie pas que les jobs partagent
les fichiers générés pendant leur exécution.

build charge donc le fichier comme artefact, puis verify le télécharge.
La dépendance needs: build garantit que verify attend la réussite de build.

## Q6. Pourquoi checkout doit-il précéder l'appel de l'action locale ?

L'appel ./.github/actions/build-info recherche l'action dans l'espace
de travail du runner.

actions/checkout récupère les fichiers du dépôt, notamment action.yml.
Il doit donc s'exécuter avant cet appel local pour que GitHub puisse
trouver et charger l'action.

## Q7. Pourquoi préciser shell: bash dans une action composite ?

Chaque étape run d'une action composite doit préciser l'interpréteur
utilisé pour exécuter ses commandes.

L'attribut shell est donc obligatoire pour ces étapes. Dans ce TP,
shell: bash indique que les commandes doivent être exécutées avec Bash.

C'est la présence de shell qui est obligatoire : Bash est le choix
retenu ici, mais d'autres interpréteurs peuvent être utilisés.

## Déclenchement du workflow

Le workflow conserve workflow_dispatch et ajoute un déclenchement au
push sur feature/tp-actions pour permettre sa vérification directement
sur la branche du rendu.

Les jobs build et docker-hello peuvent s'exécuter en parallèle.
Le job verify attend build avant de télécharger build-report.

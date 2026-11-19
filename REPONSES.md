
# TP6 — Variables d'environnement, Inputs et Outputs

## Q1. Pourquoi la variable est-elle disponible uniquement dans les étapes suivantes ?

Une écriture dans GITHUB_ENV ajoute une ligne dans un fichier traité par
GitHub à la fin de l'étape. Elle ne modifie pas directement l'environnement
du shell en cours.

La variable devient donc disponible dans les étapes suivantes du même
job. Pour l'utiliser immédiatement dans l'étape qui l'écrit, il faudrait
également la définir dans le shell de cette étape.

## Q2. Que constate-t-on dans verify-env ? Quelle est la portée de GITHUB_ENV ?

Dans cette configuration, verify-env affiche APP_NAME=[] : la variable
définie dans build n'est pas disponible dans cet autre job.

GITHUB_ENV permet de transmettre des variables aux étapes suivantes du
même job. Sa portée ne s'étend pas aux autres jobs du workflow.

Même si verify-env attendait build avec needs: build, cela ne suffirait
pas à lui transmettre APP_NAME.

## Q3. Que se passe-t-il avec > au lieu de >> dans GITHUB_OUTPUT ?

Avec deux commandes d'écriture successives, > remplace le contenu du
fichier à chaque écriture. La seconde commande efface donc l'output
enregistré par la première.

L'opérateur >> ajoute les nouvelles lignes à la suite du contenu
existant. Il permet de conserver plusieurs outputs dans la même étape.

## Q4. Pourquoi GITHUB_ENV ne suffit-il pas pour communiquer entre jobs ?

GITHUB_ENV transmet des variables entre les étapes d'un même job.

Pour transmettre la version à deploy, l'étape Compute full version
écrit un output dans GITHUB_OUTPUT. Son identifiant version permet de
récupérer cette valeur avec steps.version.outputs.full_version.

La valeur est ensuite déclarée comme output du job build. Le job deploy
déclare needs: build et la lit avec needs.build.outputs.full_version.

Le transfert dépend donc de l'output de l'étape, de sa déclaration au
niveau du job et de la dépendance needs.

## Q5. Que se passe-t-il si needs: build est retiré de deploy ?

Le job deploy n'attend plus build et peut démarrer en parallèle.

Le job build ne faisant plus partie de ses dépendances, deploy ne peut
plus récupérer sa version avec needs.build.outputs.full_version.
Dans ce workflow, cette expression donne une valeur vide.

Le job peut tout de même réussir, car echo n'échoue pas simplement
parce que la version est vide.

## Exécution sur la branche

Un déclenchement automatique au push a été ajouté pour vérifier les jobs
directement sur feature/tp-env. Cette exécution utilise staging et 1.0.0.

Le déclencheur manuel workflow_dispatch et ses trois inputs sont également
définis. Lors d'un push, aucun input debug n'est fourni : l'étape de debug
est donc ignorée. Lors d'un lancement manuel, elle dépend de la case debug.

L'exécution automatique ne constitue pas une vérification du formulaire
de lancement manuel.

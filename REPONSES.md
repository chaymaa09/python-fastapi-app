# Réponses aux questions
## Q1
GitHub Actions lit le fichier GITHUB_ENV à la fin de la step qui l'a modifié puis il injecte les variables dans l'environnement des steps suivantes, donc la step actuelle ne voit donc pas immédiatement les variables qu'elle vient d'écrire.

## Q2
Dans le job verify-env, la variable APP_NAME n'est pas définie dans ce job, donc GITHUB_ENV possède une portée limitée au job dans lequel la variable est définie. Il permet de partager des variables entre les différentes steps d'un même job, mais pas directement entre plusieurs jobs.

## Q3
L'opérateur > remplace le contenu existant du fichier, tandis que >> ajoute du contenu à la fin du fichier. Donc, si deux outputs sont définis successivement avec >, le deuxième va écraser le premier.

## Q4
Parce que GITHUB_ENV permet uniquement de transmettre des variables d'environnement entre les steps d'un même job et il ne permet pas de partager directement des variables entre différents jobs, alors que GITHUB_OUTPUT permet de transmettre explicitement des données entre plusieurs jobs.

## Q5
GitHub Actions considère que le job deploy ne dépend pas du job buildet les deux jobs peuvent donc s'exécuter en parallèle et par conséquent, le contexte needs.build.outputs  ne sera pas disponible car build n'est plus une dépendance directe de deploy.  
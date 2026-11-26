# Réponses aux questions du TP-06
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


# Réponses aux questions du TP-07
## Q1
actions/checkout fonctionne avec des valeurs par défaut pertinentes, alors que actions/setup-node nécessite de savoir quelle version de Node.js installer. with sert à passer les paramètres (inputs) à l'action et installer un environnement Node.js spécifique sur la machine virtuelle.

## Q2
Car chaque job s'exécute dans un environnement d'exécution indépendant, alors les fichiers créés dans le job build ne sont donc pas automatiquement disponibles dans le job verify  donc il faut utiliser upload-artifact pour sauvegarder le fichier et download-artifact pour le récupérer dans l'autre job.  

## Q6
Une action locale est stockée dans le dépôt GitHub et pour pouvoir l'utiliser, ses fichiers doivent être présents dans l'espace de travail de la machine virtuelle.
Alors actions/checkout permet de récupérer le contenu du dépôt et donc de rendre accessible le fichier .github/actions/build-info/action.yml.

## Q7
chaque step utilisant run doit préciser explicitement le shell à utiliser.
Cela permet à GitHub Actions de savoir avec quel interpréteur exécuter les commandes.

# Réponses aux questions du TP-08
## Q1
la matrice combine 2 systèmes d'exploitation avec 3 versions de Python, ce qui donne 6 combinaisons possibles. Donc 6 jobs seront créés et exécutés en parallèle.
Les combinaisons sont :
- Ubuntu / Python 3.11
- Ubuntu / Python 3.12
- Ubuntu / Python 3.13
- Windows / Python 3.11
- Windows / Python 3.12
- Windows / Python 3.13

## Q2
6 jobs au total, les 6 combinaisons initiales moins la combinaison Windows/Python 3.11, puis ajout de Ubuntu/Python 3.14.

## Q3
Si on tente d'exclure Python 3.14 dans exclude, cette exclusion ne supprimera pas la combinaison ajoutée par include car GitHub Actions applique exclude avant include. La combinaison Python 3.14 sera donc ajoutée après l'exclusion.

## Q4
- Avec fail-fast= true : 
un seul job a échoué : Ubuntu avec Python 3.13. 
Les cinq autres jobs ont été annulés à la suite de cet échec. Aucun job n'a été marqué comme réussi sur cette exécution.

- Avec fail-fast= false :
2 jobs ont échoué : windows avec Python 3.13 et Windows avec Python 3.12. les jobs ubuntu ont réussi. 
Donc 4 jobs ont été marqués comme réussis sur cette exécution.

lorsque l'objectif est d'arrêter les traitements dès qu'une erreur importante apparaît pour gagner du temps et de réduire les coûts d'exécution, fail-fast: truepermet à annuler les autres jobs de la matrice qui sont encore en cours ou en attente dès qu'un job échoue.
pour les tests de compatibilité multi-plateformes et diagnostics complets sur toutes les configurations, fail-fast:false permet aux jobs continuent leur exécution, même si certains échouent.

## Q5
max parallel permet de :
- limiter le nombre de jobs simultanés et de contrôler la consommation des ressources et des runners GitHub Actions.

- éviter de lancer trop de jobs simultanément pour réduire la surcharge de l'infrastructure CI/CD et de mieux répartir les ressources disponibles.

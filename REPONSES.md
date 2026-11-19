# Réponses aux questions
## Q1
GitHub Actions lit le fichier GITHUB_ENV à la fin de la step qui l'a modifié puis il injecte les variables dans l'environnement des steps suivantes, donc la step actuelle ne voit donc pas immédiatement les variables qu'elle vient d'écrire.


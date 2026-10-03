# Préparation avant la séance — hors des 4 h 30

Public Bac+4/5, débutant à intermédiaire en cloud. Savoir créer un fichier, lancer une commande, lire une erreur et utiliser les bases Java (classe, méthode, exception). Installer JDK **17**, Maven **3.9.x**, Git, `curl`, un éditeur et Bash (Linux/macOS ou WSL2 sous Windows). La syntaxe des instructions suppose Bash ; utiliser WSL2 plutôt que traduire les commandes en PowerShell pendant la séance.

Disposer d'un compte GitHub avec droit de créer son propre dépôt, Actions autorisé et accès réseau à GitHub et Maven Central. Le dépôt distribué appartient à `pilotcrew-io` ; les preuves et essais sont réalisés dans votre propre copie. Le formateur vérifie les limitations de quota et les autorisations de l'organisation avant la séance.

Contrôles de préparation :

```bash
java -version
javac -version
mvn -version
git --version
curl --version
```

Résultat attendu : Java et javac annoncent 17, Maven utilise ce même JDK. Faire résoudre à l'avance les dépendances de l'atelier via un projet de test fourni par l'encadrant ; le premier téléchargement peut être lent. Ne pas lire le corrigé pour préparer l'exercice.

Une difficulté d'installation n'est pas une difficulté de développement : obtenir un poste prêt avant le démarrage. Préparer un dossier de preuves local expurgées et une fenêtre Bash supplémentaire pour les requêtes HTTP.

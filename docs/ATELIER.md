# Atelier 01 — Une API Java que la CI sait refuser

Durée : **4 h 30**, prérequis terminés. Travail individuel ou binôme avec permutation conducteur/relecteur après les tests. Mission : l'équipe reçoit régulièrement des modifications Java qui compilent sur un poste mais cassent le service. Construire une API fictive et une chaîne de preuves permettant une revue avant fusion.

## Contrat à respecter

| Requête | Résultat demandé |
|---|---|
| GET /health | 200, JSON `{"status":"UP"}` |
| GET /api/quotes | 200, tableau JSON avec DEMO, prix 42.0, devise EUR, fictional true |
| GET sur un chemin inconnu ou /health/private | 404 et erreur JSON |
| POST /api/quotes | 405, en-tête Allow: GET |

Toutes les réponses portent Content-Type application/json; charset=utf-8. Le serveur écoute toutes les interfaces sur le port local 8080 par défaut ; la configuration peut changer le port. Ne pas ajouter base de données, appel financier ou authentification. La liste de cotations est fixe : l'objectif est la livraison et la vérification.

## Déroulé — 270 minutes

| Temps | Travail | Point de contrôle |
|---|---|---|
| 00:00–00:20 · 20 min | Lire contrat, dessiner source → test → JAR → CI, créer copie personnelle et journal | Contrat reformulé ; poste prêt |
| 00:20–01:10 · 50 min | Construire pom et serveur Java17 ; séparer démarrage, routage et arrêt | /health et /api/quotes répondent localement |
| 01:10–02:05 · 55 min | Écrire tests utiles, port éphémère, fermeture après test ; traiter 404/405 | Tests passent et détectent une régression volontaire |
| 02:05–02:50 · 45 min | Empaqueter JAR exécutable ; lancer hors IDE ; documenter version et port | java -jar démarre et curl valide le contrat |
| 02:50–03:40 · 50 min | Créer CI Actions sur main et PR : JDK17, Maven, rapports toujours, JAR si succès | Run vert avec deux artefacts téléchargeables |
| 03:40–04:10 · 30 min | Créer branche d'incident, introduire mauvais statut, ouvrir PR, observer rouge, réparer | Même PR rouge puis verte et rapport d'erreur lisible |
| 04:10–04:30 · 20 min | Revue, preuves, nettoyage processus et synthèse | Livraison reproductible et barème renseigné |

Les téléchargements et attentes CI sont inclus dans les plages. Au premier blocage réseau de plus de 10 minutes, l'encadrant active sa copie de secours ; documenter une preuve locale sans inventer un run GitHub.

## Étape 1 — Comprendre avant d'écrire

Créer son dépôt de travail à partir de cette copie pédagogique et y ouvrir une branche de réalisation. Compléter travail/REPONSES.md. Quel est le lien entre une réponse HTTP et une assertion ? Quelle vérification manquerait si l'on testait uniquement que le serveur démarre ? Écrire les cas nominal, chemin inconnu, méthode interdite et préfixe trompeur avant le serveur.

## Étape 2 — Construire le service

Créer l'arborescence Maven standard sous src/main/java et src/test/java. Choisir une classe App contenant un main, un serveur JDK HttpServer et un mécanisme d'arrêt. Fixer Java17 dans le build et les versions des plugins. Construire des octets UTF-8 avant le calcul de la longueur HTTP. Les contextes HttpServer font une correspondance par préfixe : vérifier explicitement les chemins exacts. Les handlers doivent fermer la réponse même en cas d'erreur d'écriture.

Checkpoint : dans une seconde fenêtre, employer curl sur le contrat, noter code, en-tête et corps. N'exposer aucune donnée personnelle dans le service ou les logs.

## Étape 3 — Prouver le comportement

Écrire au moins deux tests unitaires de choix/validation du port et six tests de contrat HTTP. Utiliser un port attribué automatiquement dans les tests ; lire le port réellement lié au serveur. Chaque test démarrage doit être accompagné d'un arrêt. Aucun délai fixe de plusieurs secondes nécessaire : start rend le serveur disponible. Les requêtes de test ont un timeout. Tester la présence d'un en-tête et le corps, pas seulement l'absence d'exception.

Commandes de validation autorisées :

```bash
mvn --batch-mode --no-transfer-progress clean verify
java -jar target/quote-api.jar
curl -i http://localhost:8080/health
curl -i http://localhost:8080/api/quotes
```

Ces commandes ne donnent pas l'implémentation. Lire les rapports dans target/surefire-reports. Expliquer pourquoi un test 404 échoue si le handler renvoie 200 pour tous les chemins.

## Étape 4 — Livrer un binaire

Configurer le manifeste Main-Class et un nom de JAR constant. Lancer le JAR depuis un terminal indépendant de l'IDE. Noter qu'un JAR sans dépendance d'exécution suffit ici ; JUnit appartient au scope test. Comparer test, package et verify. Ne pas utiliser skipTests pour rendre une livraison verte.

## Étape 5 — Automatiser la preuve

Écrire .github/workflows/ci.yml dans son dépôt de travail. Définir déclencheurs main, pull_request et lancement manuel ; un runner hébergé, un timeout, permissions minimales. Installer explicitement Java17. Exécuter clean verify en mode batch. Publier les rapports même lorsque les tests échouent, puis le JAR seulement en cas de succès. Prévoir rétention courte et noms d'artefacts uniques. Choisir les versions des actions dans les sources officielles.

Dans Actions, relier le run au SHA et télécharger le JAR et le XML de tests. Expliquer la différence entre cache des dépendances et artefact livré. Les rapports sont conservés en fichiers ; leur publication n'ajoute pas automatiquement un tableau de tests dans l'interface GitHub.

## Étape 6 — Une pull request doit pouvoir échouer

Sur une branche incident, remplacer intentionnellement un statut contractuel dans le code. Ouvrir une PR vers main et conserver lien du run rouge et assertion concernée. Corriger sur cette même branche, vérifier localement puis pousser. La PR doit devenir verte. Une règle exigeant le job build est un exercice de gouvernance si les droits et l'offre GitHub le permettent ; sinon documenter cette limite et effectuer une revue manuelle. Aucun élève n'approuve sa propre revue : le binôme ou l'encadrant relit le résultat.

## Étape 7 — Rendre un résultat défendable

Compléter le journal avec les preuves du barème. Arrêter le serveur (Ctrl+C), vérifier qu'aucun processus de cet atelier n'écoute encore. Rédiger cinq lignes sur ce qu'un run vert ne prouve pas : sécurité, charge, disponibilité Azure et qualité du produit restent d'autres sujets.

## Livrables et extension

Dépôt contenant source, tests, pom, workflow et journal ; lien de PR rouge puis verte ; JAR exécutable ; rapports. Extension hors temps évalué : SHA d'actions vérifiés ou matrice Java17/21. Le corrigé est un dépôt distinct communiqué après la restitution.

# Journal individuel à compléter

## Identité du livrable

- Auteur / binôme : Aurélia
- Date et durée réelle : 05/10/2026
- URL du dépôt personnel : https://github.com/auravelo08/azdevops-01-java-ci-eleve
- SHA de la version livrée :

## Décisions

| Décision | Alternatives | Justification | Vérification |
|---|---|---|---|
| À compléter | | | |

## Preuves expurgées

| Critère du barème | Commande ou écran utilisé | Résultat observé | Fichier ou URL de preuve |
|---|---|---|---|
| À compléter | | | |

## Incident analysé

Symptôme :

Hypothèse :

Diagnostic qui distingue les causes :

Résolution et nouvelle vérification :

## Retour critique

Ce que les vérifications prouvent :

Ce qu'elles ne prouvent pas :

Prochaine amélioration proposée et raison :

Étape 1 - Comprendre avant d'écrire : 
Quel est le lien entre une réponse HTTP et une assertion ? Une assertion vérifie la réponse à la suite de la requête HTTP.
Quelle vérification manquerait si l'on testait uniquement que le serveur démarre ? On vérifie seulement que le serveur démarre sans vérifier les requêtes effectuées et réponses reçues. 

- cas nominal (tout se passe comme prévu) : GET /health 200 > JSON {"status":"UP"}
- chemin inconnu / inexistant : GET/chemininconnu > 404 et erreur JSON
- méthode interdite : POST /api/quotes > 405, en-tête Allow: GET
- préfixe trompeur : GET/health/private > 404 et erreur JSON

Schéma de compréhension : 
source : crée le code Java : 
- Si Phase de test OK : le JAR (fichier qui contient l'appli mais qui est lancé par une commande) peut etre lancé (donc l'appli sera lancé) 
- Si test PAS OK : on revoit le code l'appli, pas de JAR.
CI en fonction des tests (les tests sont relancés à chaque modif ensuite le fichier JAR est fait) :
- SI OK : le fichier JAR est produit et affiche vert 
- SI PAS OK : pas de JAR et affiche rouge et les modifications ne sont pas prises en compte donc pas fusionnée à la branch main

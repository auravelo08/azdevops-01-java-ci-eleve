# Évaluation 01 — 20 points

| Critère | Points | Preuve attendue |
|---|---:|---|
| Contrat HTTP exact : 200, JSON, 404, 405 et Allow | 4 | Requêtes expurgées et code routage |
| Tests utiles : nominal et négatif, port, ressources libérées | 4 | Tests et rapports avec au moins 8 cas ; mutation détectée |
| Maven Java17 et JAR autonome | 3 | pom, clean verify, exécution java -jar hors IDE |
| CI main/PR, permissions minimales et JDK explicite | 3 | Workflow et URL d'un run lié au SHA |
| Rapports après échec et JAR après succès | 2 | Rapport run rouge et artefact JAR run vert |
| PR rouge puis verte, revue expliquée | 2 | Même PR, deux runs, commentaire de revue |
| Journal, diagnostic et limites | 2 | travail/REPONSES.md complété et arrêt serveur |
| **Total** | **20** | |

La présence d'un fichier sans preuve de comportement ne vaut pas tous les points. Sur panne plateforme documentée par l'encadrant, les 5 points CI/artefacts peuvent être évalués sur revue du workflow et exécution locale des mêmes étapes ; le journal doit marquer explicitement « GitHub non exécuté ». Ne jamais fournir de captures fabriquées.

Acceptation : clone propre, prérequis présents, clean verify réussi, JAR lançable, endpoints conformes, échec volontaire identifié, aucune information sensible suivie. Fournir SHA, versions des outils, URLs de PR/runs et emplacement des artefacts. Les liens d'artefacts expirent : conserver copie expurgée des rapports pour la restitution.

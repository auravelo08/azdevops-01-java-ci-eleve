# Dépannage 01

Lire d'abord la première erreur, conserver commande, SHA et sortie expurgée. Ne pas rendre la CI verte en retirant un test.

| Symptôme | Cause plausible | Diagnostic discriminant | Résolution attendue |
|---|---|---|---|
| release version 17 not supported | Maven utilise JDK ancien | Comparer mvn -version et javac -version | Corriger JAVA_HOME du poste, rouvrir terminal |
| no main manifest attribute | Main-Class absent | Inspecter META-INF/MANIFEST.MF via jar tf / jar xf dans dossier temporaire | Configurer plugin jar et refaire clean verify |
| Tests run: 0 | Mauvais nom ou dépendance JUnit | Lire Surefire et vérifier AppTest dans src/test/java | Dépendance Jupiter, nom Test et failIfNoTests |
| Address already in use | Serveur précédent actif | Identifier processus écoutant 8080 avec outil du poste | Arrêter le processus identifié ou changer PORT |
| Test bloque | Pas de timeout ou serveur jamais démarré | Lire la ligne requête et état des threads | start avant requête, timeout et arrêt afterEach |
| /health/private renvoie 200 | Correspondance préfixe HttpServer | Tester chemin exact et préfixe | Vérifier explicitement URI.getPath |
| Run local vert, CI rouge | Fichier non suivi / version JDK | Comparer git status, SHA du run et version du runner | Ajouter source manquante, déclarer JDK17 |
| Aucun artefact rapport | Échec avant Surefire ou mauvais path | Lire phase atteinte et contenu target | Réparer erreur initiale puis chemin |
| Actions attend une approbation | PR de fork ou règle organisation | Lire bannière du run | Encadrant autorise selon sa politique, sans secret |
| Pas de protection de branche | Offre/droits insuffisants | Vérifier réglages et rôle GitHub | Revue manuelle tracée et limite documentée |
| Résolution dépendances échoue | Proxy, Central indisponible | Lire HTTP et tester réseau autorisé | Régler réseau avec encadrant, ne pas copier jeton |

Pour le serveur local, Ctrl+C déclenche le hook d'arrêt. Ne pas utiliser un arrêt global de toutes les JVM : un autre atelier peut partager le poste.

# Mémos 01 — Pourquoi chaque preuve existe

## CI et revue

L'intégration continue exécute les mêmes contrôles après une modification. Le runner est une machine éphémère : ce qui fonctionnait grâce à un fichier caché sur un poste ne doit pas devenir une dépendance implicite. Déclarer JDK, plugins et dépendances rend le build explicable. La PR relie intention, diff, discussions et résultat. Un job rouge protège seulement si l'équipe le prend en compte, idéalement via une règle de branche disponible dans son offre GitHub.

La CI construit une proposition de changement ; elle ne doit pas recevoir les identités cloud pour ce premier projet. Un contributeur peut modifier le code exécuté par les tests. Le minimum `contents: read`, l'absence de secrets et un événement pull_request limitent l'impact. Ne pas utiliser pull_request_target pour contourner une limitation de fork.

## Maven : phases et responsabilités

| Élément | Rôle | Erreur fréquente |
|---|---|---|
| src/main/java | Code embarqué | Mettre les tests dans le JAR |
| src/test/java | Tests exécutés par Surefire | Tests non découverts faute de nom Test |
| compile | Compiler le code principal | Croire que compiler prouve le comportement |
| test | Exécuter les tests unitaires | Désactiver les assertions qui échouent |
| package | Construire le JAR après les phases précédentes | JAR présent mais main absent |
| verify | Aller jusqu'aux contrôles de vérification | Le confondre avec un déploiement |
| clean | Effacer les sorties précédentes | Réutiliser une ancienne classe compilée |

Maven applique les phases précédentes de son cycle de vie. `clean verify` combine nettoyage et construction vérifiée. Le scope test évite d'envoyer JUnit en production. Ici l'application s'appuie uniquement sur le JDK ; le JAR ordinaire avec manifeste suffit. Un framework avec dépendances d'exécution demanderait une autre stratégie de packaging.

## Tests : unité, contrat et régression

Un test unitaire du port vérifie une fonction en mémoire. Un test de contrat HTTP lance un serveur local et vérifie un échange réel ; il reste rapide mais nécessite ressources et arrêt. Un test de fumée sur le JAR vérifie le packaging et le démarrage, des risques que le test sur les classes ne couvre pas.

Le test compare observé et attendu. Une assertion exacte de corps détecte un champ supprimé. Le port 0 dans un test laisse le système choisir une adresse libre et évite collisions entre suites. Un timeout transforme un blocage réseau en échec diagnosable. Les tests ne doivent pas attendre indéfiniment un service externe ; les cotations sont donc fictives et locales.

## HTTP et exploitation

200 indique que la route a répondu conformément au cas nominal ; 404 représente un chemin inexistant ; 405 une méthode interdite et Allow annonce la méthode acceptée. Ces codes sont une interface contractuelle pour consommateurs et sondes. Une sonde health dit que le processus répond, pas que toutes les dépendances d'un futur produit sont saines. Les en-têtes et l'encodage doivent être cohérents avec les octets réellement écrits.

HttpServer associe les contextes par préfixe. Sans contrôle de chemin, /health/private pourrait atteindre le handler health et produire une réponse trompeuse. Cette subtilité justifie un test spécifique. Fermer l'exchange termine la réponse ; arrêter serveur et pool libère threads et sockets.

## Cache, artefact et traçabilité

Un cache Maven réduit les téléchargements. Il peut être absent sans invalider le build. Un artefact contient une sortie que l'on veut récupérer : JAR, rapport ou documentation. On doit pouvoir reconstruire le JAR depuis les sources et versions déclarées, mais on conserve la sortie validée pour la revue. Nommer avec SHA établit la relation source → run → binaire ; un nom seul ne garantit ni provenance forte ni signature.

Les rapports XML/TXT de Surefire expliquent test, durée et échec. Publier avec always permet l'analyse après rouge. Un échec avant lancement des tests ne produit pas ces rapports : warn est alors honnête. Le JAR n'est publié que si verify réussit. Rétention courte réduit l'encombrement mais impose d'enregistrer les preuves utiles avant expiration.

## Lecture d'une panne CI

Chercher la première erreur causale : installation JDK, résolution Maven, compilation, assertion puis upload. Une dernière étape manquante est souvent la conséquence d'un échec précédent. Comparer le SHA et la commande locale avant d'accuser le runner. Un vert ne prouve pas couverture exhaustive, résistance aux attaques ou performance sous charge.

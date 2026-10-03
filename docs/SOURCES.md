# Sources officielles et décisions techniques

Vérification documentaire : **3 octobre 2026**. Les versions ci-dessous sont des choix reproductibles du laboratoire, pas une promesse d'être les dernières. Vérifier les références avant une nouvelle promotion. Pages consultées :

| Source primaire | Utilisation dans le laboratoire |
|---|---|
| [Oracle — HttpServer Java 17](https://docs.oracle.com/en/java/javase/17/docs/api/jdk.httpserver/com/sun/net/httpserver/HttpServer.html) | serveur HTTP embarqué, contexte et arrêt |
| [Maven — cycle de vie](https://maven.apache.org/guides/introduction/introduction-to-the-lifecycle.html) | test, package, verify et clean |
| [Maven — Surefire](https://maven.apache.org/surefire/maven-surefire-plugin/) | découverte des tests et rapports XML/TXT |
| [JUnit — guide](https://docs.junit.org/5.12.2/user-guide/) | assertions, before/after et tests Jupiter |
| [GitHub — Java avec Maven](https://docs.github.com/en/actions/tutorials/build-and-test-code/java-with-maven) | runner, installation JDK et cache Maven |
| [actions/setup-java](https://github.com/actions/setup-java) | major v6 documentée, Java17 et permissions contents:read |
| [actions/checkout](https://github.com/actions/checkout) | major v7 documentée pour runner hébergé |
| [actions/upload-artifact](https://github.com/actions/upload-artifact) | major v7 documentée, rapports et durée de rétention |
| [GitHub — règles de branches](https://docs.github.com/en/repositories/configuring-branches-and-merges-in-your-repository/managing-protected-branches/about-protected-branches) | statut CI requis, disponibilité selon offre et dépôt |

Le projet cible Java17, JUnit5.12.2, compiler3.14.0, surefire3.5.3 et jar3.4.2. Les majors d'Actions simplifient l'entretien ; un dépôt professionnel fixe des SHA complets vérifiés puis organise leurs mises à jour. Aucun SHA fictif. Les commandes sont prévues pour GitHub.com et runner hébergé ubuntu-24.04 ; GitHub Enterprise peut supporter d'autres majors.

# Serveur GraphQL — SEG3502 Lab 8

Le serveur utilise Spring Boot 3.3.1, Kotlin 1.9.24, Java 21, Spring for GraphQL et Spring Data MongoDB.

Démarrez MongoDB depuis la racine du dépôt avec `docker compose up -d`, puis dans ce dossier :

Windows PowerShell : `\gradlew.bat bootRun`

macOS/Linux : `./gradlew bootRun`

Le point de terminaison GraphQL est `http://localhost:9000/graphql` et GraphiQL est disponible à `http://localhost:9000/graphiql`.
Voir le README à la racine pour les prérequis et toutes les étapes de démarrage.

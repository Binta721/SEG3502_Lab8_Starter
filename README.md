# SEG3502 — Laboratoire 8 — Employee GraphQL

Ce dépôt contient le client Angular fourni pour le laboratoire et le serveur Spring Boot/Kotlin complété pour les opérations GraphQL utilisées par ce client.

## Prérequis

Installez les éléments suivants sur chaque ordinateur :

- **Java JDK 21** (pas seulement un JRE) ;
- **Node.js 20.11+ ou 22+** et npm ;
- **Docker Desktop** (pour lancer MongoDB avec Docker Compose) ;
- Git (pour cloner le dépôt).

Le projet utilise le Gradle Wrapper inclus : il n'est pas nécessaire d'installer Gradle séparément. Le premier lancement télécharge les dépendances et peut prendre quelques minutes.

Vérifiez les installations dans un terminal :

```bash
java -version
node -v
npm -v
docker --version
docker compose version
```

Java doit afficher une version majeure 21. Node doit être compatible avec Angular 18 (Node 20.11+ ou 22+).

## 1. Récupérer le projet

```bash
git clone URL_DU_DEPOT
cd SEG3502_Lab8_Starter
```

Remplacez `URL_DU_DEPOT` par l'URL HTTPS de votre dépôt GitHub.

## 2. Démarrer MongoDB

À la racine du projet, dans le dossier qui contient `docker-compose.yml` :

```bash
docker compose up -d
```

Vérifiez que le conteneur fonctionne :

```bash
docker compose ps
```

La base est persistée dans un volume Docker nommé `employee-mongo-data`. Pour arrêter MongoDB sans supprimer les données :

```bash
docker compose down
```

Ne lancez pas `docker compose down -v` sauf si vous voulez supprimer les données de la base.

## 3. Démarrer le serveur GraphQL

Ouvrez un terminal dans `server`.

Windows PowerShell :

```powershell
.\gradlew.bat clean build
.\gradlew.bat bootRun
```

macOS/Linux :

```bash
./gradlew clean build
./gradlew bootRun
```

Le serveur écoute sur `http://localhost:9000`.

- GraphQL : `http://localhost:9000/graphql`
- Interface GraphiQL : `http://localhost:9000/graphiql`

Gardez ce terminal ouvert pendant l'utilisation de l'application.

## 4. Démarrer le client Angular

Ouvrez un deuxième terminal dans `client` :

```bash
npm ci
npm start
```

Ouvrez ensuite `http://localhost:4200` dans votre navigateur.

Le client fourni est configuré pour appeler `http://localhost:9000/graphql`. Si le serveur tourne sur le même ordinateur, aucune modification de l'URL n'est nécessaire.

## 5. Vérification rapide de l'API

Avec le serveur démarré, exécutez cette commande PowerShell dans un autre terminal :

```powershell
Invoke-RestMethod -Uri "http://localhost:9000/graphql" -Method POST -ContentType "application/json" -Body '{"query":"{ employees { id name dateOfBirth city salary gender email } }"}'
```

Une liste vide est normale au premier démarrage : elle signifie que l'API répond et que la collection ne contient pas encore d'employés.

## Dépannage

- **Port 27017 occupé** : un autre MongoDB utilise déjà ce port. Arrêtez l'autre service ou adaptez le port dans `docker-compose.yml` et l'URI MongoDB dans `server/src/main/resources/application.properties`.
- **Port 9000 ou 4200 occupé** : arrêtez l'application qui utilise le port ou modifiez sa configuration.
- **Gradle ne trouve pas Java 21** : vérifiez `java -version` et que `JAVA_HOME` pointe vers un JDK 21. Fermez puis rouvrez le terminal après avoir modifié les variables d'environnement.
- **Le client ne joint pas GraphQL** : vérifiez que MongoDB et le serveur Spring Boot sont démarrés avant Angular.

## Structure

```text
.
├── client/                 # Application Angular 18 fournie
├── server/                 # Spring Boot + Kotlin + Spring GraphQL + MongoDB
├── docker-compose.yml      # MongoDB local
└── README.md
```

## Opérations GraphQL implémentées

- `employees` : récupère tous les employés depuis MongoDB.
- `newEmployee(createEmployeeInput: ...)` : crée un employé et renvoie l'employé enregistré.

Le schéma est défini dans `server/src/main/resources/graphql/employee.graphqls`.

# 🚀 Projet : Dockerisation d'une Application E-Commerce (Microservices)

Ce projet consiste en la conteneurisation et l'orchestration d'une application e-commerce basée sur une architecture microservices (Express.js pour les services back-end et un front-end dédié), le tout routé via un reverse proxy Nginx.

## 🏗 Architecture du projet

L'application est découpée en plusieurs services indépendants :
- **Reverse Proxy** (Nginx) : Point d'entrée unique de l'application, redirigeant les requêtes vers les bons services.
- **Frontend** : Interface utilisateur de l'application e-commerce.
- **Auth Service** (Express.js) : Gestion de l'authentification et des utilisateurs.
- **Product Service** (Express.js) : Gestion du catalogue de produits.
- **Order Service** (Express.js) : Gestion des commandes.

---

## 🛠 1. Étapes pour construire et exécuter les conteneurs

Assurez-vous d'avoir [Docker](https://docs.docker.com/get-docker/) et [Docker Compose](https://docs.docker.com/compose/install/) installés sur votre machine.

### Lancement par défaut (Mode Développement)
Pour construire les images et démarrer tous les conteneurs en arrière-plan :

```bash
docker-compose up --build -d
```

Pour vérifier l'état des conteneurs :
```bash
docker-compose ps
```

Pour arrêter et supprimer les conteneurs, réseaux et volumes associés :
```bash
docker-compose down -v
```

---

## 🌍 2. Configurations spécifiques à chaque environnement

Le projet utilise plusieurs fichiers Docker Compose et fichiers d'environnement (`.env`) pour s'adapter au contexte d'exécution.

### Environnement de Développement (`.env.development`)
- **Fichiers utilisés** : `docker-compose.yml` et `docker-compose.override.yml`.
- **Commande de lancement** : 
  ```bash
  docker-compose -f docker-compose.yml -f docker-compose.override.yml up --build -d
  ```
- **Spécificités** : 
  - Les volumes locaux sont montés dans les conteneurs pour permettre le *Hot-Reload* (les modifications de code sont appliquées en temps réel sans avoir à reconstruire les images).
  - Les variables d'environnement exposent des configurations adaptées au développement.

### Environnement de Production (`.env.production`)
- **Fichiers utilisés** : `docker-compose.yml` et `docker-compose.prod.yml`.
- **Commande de lancement** : 
  ```bash
  docker-compose -f docker-compose.yml -f docker-compose.prod.yml up --build -d
  ```
- **Spécificités** :
  - Utilisation d'images optimisées.
  - Ports exposés limités au strict nécessaire (ex: seul Nginx expose les ports 80/443 vers l'extérieur).

---

## 🧪 3. Commandes pour tester les services

Des scripts ont été mis en place pour faciliter l'exécution des tests sur les différents microservices. 

Vous pouvez exécuter l'ensemble des tests via le script dédié à la racine :
```bash
./scripts/run-tests.sh
```

Alternativement, pour exécuter les tests directement via Docker Compose :
```bash
docker compose build --build-arg TARGET=test
```

---

## 🔒 4. Bonnes pratiques appliquées

Ce projet a été conçu en respectant les standards de l'industrie pour la conteneurisation :

1. **Multi-stage Builds** : Les `Dockerfile` utilisent des constructions en plusieurs étapes pour séparer l'environnement de build (installation des dépendances, compilation) de l'environnement d'exécution. Cela réduit considérablement la taille finale des images de production et la surface d'attaque.
2. **Multi Environnements** : Les `Dockerfile` ont plusieurs environnements : `base`, `développement`, `test`, `build` et `production`.
3. **Sécurité (Utilisateur non-root)** : Les conteneurs n'exécutent pas les processus en tant que `root`. Un utilisateur spécifique (ex: `node`) est défini dans les `Dockerfile` (`USER node`) pour limiter les droits en cas de compromission.
4. **Fichiers `.dockerignore`** : Présents dans chaque service pour éviter d'envoyer le répertoire `node_modules`, les fichiers de logs ou les fichiers secrets locaux (comme les fichiers `.env` ou `jwt_secret.txt`) dans le contexte de build Docker.
5. **Gestion des Secrets et Variables d'Environnement** : L'utilisation de fichiers `.env` sépare la configuration du code source. Des fichiers distincts gèrent les secrets JWT et les identifiants de bases de données selon l'environnement.
6. **Reverse Proxy Nginx** : Permet de centraliser le routage, d'isoler les ports internes des services back-end de l'hôte, et de préparer le terrain pour le load-balancing ou la terminaison SSL.
7. **Sous-réseaux Docker** : Utilisation de trois sous-réseau Docker : `frontend`, `backend` et `base de données`, permettant une isolation maximale.
8. **Healhchecks** : Healthchecks sur les services critiques, qui permet leurs bon fonctionnement et leurs surveillances.
9. **Intégration et Déploiement Continus (CI/CD)** : Présence de pipelines automatisés (`.github/workflows/ci-cd.yml` et `.gitlab-ci.yml`) pour s'assurer que le code est testé et validé à chaque modification.
10. **Scans de sécurités** : Scans de sécurités des images Docker avec Trivy.

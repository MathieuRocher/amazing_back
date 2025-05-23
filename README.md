# AMAzing

**AMAzing** est une architecture de microservices en Go


## Architecture du projet

Le projet est composé de plusieurs services Dockerisés :

| Service       | Rôle principal                                 |
|---------------|------------------------------------------------|
| `gateway`     | Authentification, reverse proxy, routage, gestion des utilisatuers       |
| `form`        | Gestion des formulaires et questions           |
| `review`      | Soumission et consultation des réponses        |
| `mariadb`     | Base de données relationnelle partagée         |
| `redis`       | Cache en mémoire pour token/session management ***(Pas utilisé)*** |
| `traefik`     | Reverse proxy HTTP                              |
| `phpmyadmin`  | UI pour inspecter la base MariaDB (facultatif) |

---

## Structure des dossiers

```bash
├── amazing_gateway/
├── amazing_form/
├── amazing_review/
├── docker-compose.yml
├── .env.template
├── README.md
```

## Technologies utilisées
- Go 1.24+
- MariaDB 10.11
- Docker & Docker Compose
- Traefik v3 (reverse proxy)
- Redis
- GORM (ORM pour Go)
- JWT pour l’authentification
- Gin pour les APIs HTTP

## Fonctionnalités
- Authentification JWT
- Gestion des utilisateurs et rôles
- Création, duplication, et versionnement de formulaires
- Affectation de formulaires à des classes/cours
- Système d’évaluation (review) décentralisé
- API RESTful documentée

## Lancer le projet
### 1. Prérequis
- Docker + Docker Compose
- Un .env à la racine avec les variables suivantes (à modifier au besoin) :

```bash
# Ports
GATEWAY_PORT=8080
FORM_PORT=8081
REVIEW_PORT=8082
TRAEFIK_PORT=8088
PMA_PORT=8090

# Base de données
MARIADB_ROOT_PASSWORD=root
MARIADB_USER=amazing
MARIADB_PASSWORD=amazing
MARIADB_DATABASE=amazing_db
MARIADB_HOST=mariadb # Nom du service de bdd dans le docker compose

# Redis
REDIS_URL=redis:6379
```


### 2. Lancer les conteneurs
```bash
docker compose up --build
```

### 3. Accéder aux services
- Gateway API: http://localhost/api
- Traefik dashboard: http://localhost:${TRAEFIK_PORT}
- PhpMyAdmin: http://localhost:${PMA_PORT}

## Démarche et architecture
- Architecture modulaire avec séparation claire entre les microservices
- La communication entre services se fait via HTTP interne
- Un ORM unique (GORM) interagit avec une base MariaDB partagée
- L’usage de Traefik permet un routage souple et indépendant des ports exposés

## Démarches pas abouties
- Nous avons tenté d’intégrer un LLM (llama3 via Ollama), mais son exécution sur CPU s’est révélée trop coûteuse en ressources. Nous avons donc préféré ne pas le déployer afin d’éviter une perte de temps inutile.
- Étant donné la simplicité de notre base fonctionnelle, nous avons estimé que des tests poussés n’étaient pas très pertinents dans notre cas, ou bien trop complexes à mettre en place pour un bénéfice limité.
- Un système de cache a été intégré dans le projet, mais il n’est pas encore utilisé. Il est prêt à être activé ultérieurement si nécessaire, par exemple pour optimiser certaines requêtes.

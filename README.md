# Gestion locative API

API REST de gestion locative développée avec **Symfony** et **API Platform**.
Elle permet de gérer des biens immobiliers, des locataires, des baux, le suivi des loyers et la génération de quittances.

> Projet personnel à but pédagogique. Toutes les données sont fictives.

## Fonctionnalités

- [ ] Gestion des biens, des locataires et des baux (CRUD)
- [ ] Authentification par jeton JWT, avec deux rôles : `ROLE_MANAGER` et `ROLE_ADMIN`
- [ ] Suivi des loyers et détection des impayés
- [ ] Génération mensuelle des quittances
- [ ] Recherche, filtres et pagination sur les biens
- [ ] Documentation interactive de l'API (OpenAPI / Swagger)
- [ ] Tests unitaires et fonctionnels, avec intégration continue
- [ ] Environnement Docker prêt à l'emploi

*(Les cases sont cochées au fil de l'avancement du projet.)*

## Technologies

| Domaine | Outils |
|---|---|
| Back-end | PHP 8.x, Symfony, API Platform, Doctrine ORM |
| Base de données | MySQL (ou PostgreSQL) |
| Authentification | LexikJWTAuthenticationBundle |
| Tests | PHPUnit, Foundry (fixtures) |
| DevOps | Docker Compose, GitHub Actions |

## Modèle de données

- **User** : email, mot de passe, rôles
- **Property** (bien) : adresse, ville, type, surface, loyer, charges, statut
- **Tenant** (locataire) : nom, email, téléphone
- **Lease** (bail) : bien, locataire, date de début, date de fin, loyer mensuel, dépôt de garantie, statut
- **Payment** (loyer) : bail, période, montant dû, montant payé, date de paiement, statut
- **Receipt** (quittance) : loyer, numéro, date d'émission

Un bien a plusieurs baux au fil du temps, un bail a plusieurs loyers, et chaque loyer payé donne lieu à une quittance.

## Principaux endpoints

| Méthode | Route | Description |
|---|---|---|
| POST | `/api/login` | Obtenir un jeton JWT |
| GET, POST | `/api/properties` | Lister (avec filtres et pagination) ou créer un bien |
| GET, PUT, DELETE | `/api/properties/{id}` | Consulter, modifier ou supprimer un bien |
| GET, POST | `/api/tenants` | Lister ou créer un locataire |
| GET, POST | `/api/leases` | Lister ou créer un bail |
| GET | `/api/payments?status=late` | Lister les loyers impayés |
| POST | `/api/leases/{id}/receipts` | Générer une quittance |

La documentation complète est disponible sur `/api/docs` une fois l'application lancée.

## Installation

Prérequis : Docker et Docker Compose, Git.

### Sur macOS

Le projet a été développé sous macOS (y compris sur puce Apple Silicon).

```bash
# Installer Homebrew si besoin : https://brew.sh
brew install --cask docker      # Docker Desktop (lancer l'application une première fois)
brew install git
```

PHP, Composer et MySQL tournent dans les conteneurs : il n'est pas nécessaire de les installer sur le Mac.
Pour travailler sans Docker, `brew install php composer symfony-cli` permet de lancer le projet en local.

### Lancement

```bash
git clone https://github.com/junior-9693/gestion-locative-api.git
cd gestion-locative-api
cp .env.example .env.local
docker compose up -d
docker compose exec php composer install
docker compose exec php php bin/console doctrine:migrations:migrate
docker compose exec php php bin/console doctrine:fixtures:load
docker compose exec php php bin/console lexik:jwt:generate-keypair
```

L'API est ensuite accessible sur `http://localhost:8080/api/docs`.

## Tests

```bash
docker compose exec php php bin/phpunit
```

## Optimisation SQL

*(À compléter : description d'une requête lente, du plan d'exécution avant et après ajout d'index, et du gain mesuré.)*

## Structure du projet

```text
gestion-locative-api/
├── .github/workflows/ci.yml
├── config/
├── docker/
├── migrations/
├── src/
│   ├── Entity/
│   ├── Repository/
│   ├── State/
│   ├── Service/
│   └── DataFixtures/
├── tests/
│   ├── Unit/
│   └── Functional/
├── docker-compose.yml
├── .env.example
└── README.md
```

## Auteur

**Moussa BA** : développeur full-stack PHP / Symfony / Laravel
[GitHub](https://github.com/junior-9693)

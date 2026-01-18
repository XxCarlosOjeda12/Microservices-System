<h1 align="center">Microservices System</h1>

<p align="center">
  <i>Startup and emerging technologies management system built with a decoupled microservices architecture.</i>
</p>

<p align="center">
  <img alt="Node.js" src="https://img.shields.io/badge/Node.js-18-339933?style=flat&logo=node.js&logoColor=white" />
  <img alt="React" src="https://img.shields.io/badge/React-19-61DAFB?style=flat&logo=react&logoColor=black" />
  <img alt="PostgreSQL" src="https://img.shields.io/badge/PostgreSQL-15-4169E1?style=flat&logo=postgresql&logoColor=white" />
  <img alt="Docker" src="https://img.shields.io/badge/Docker-20.10+-2496ED?style=flat&logo=docker&logoColor=white" />
  <img alt="Nginx" src="https://img.shields.io/badge/Nginx-Alpine-009639?style=flat&logo=nginx&logoColor=white" />
  <img alt="Express" src="https://img.shields.io/badge/Express-4.x-000000?style=flat&logo=express&logoColor=white" />
  <img alt="License" src="https://img.shields.io/badge/License-MIT-yellow.svg" />
</p>

---

## Introduction

Startup and emerging technologies management system built with a decoupled microservices architecture. Each CRUD operation is implemented as an independent microservice, exposed through an Nginx API Gateway, with a responsive React frontend.

---

## Table of Contents

- [Introduction](#introduction)
- [Table of Contents](#table-of-contents)
- [Description](#description)
- [System Architecture](#system-architecture)
  - [Microservices](#microservices)
- [Project Structure](#project-structure)
- [Technology Stack](#technology-stack)
  - [Backend](#backend)
  - [Frontend](#frontend)
- [Prerequisites](#prerequisites)
  - [Installation verification](#installation-verification)
- [Installation and Execution](#installation-and-execution)
  - [Step 1: Clone the repository](#step-1-clone-the-repository)
  - [Step 2: Start services with Docker Compose](#step-2-start-services-with-docker-compose)
  - [Step 3: Verify that services are running](#step-3-verify-that-services-are-running)
  - [Step 4: Access the application](#step-4-access-the-application)
- [Environment Variables](#environment-variables)
- [API Routes](#api-routes)
  - [Base URL](#base-url)
  - [Startups Endpoints](#startups-endpoints)
    - [Example: Create startup](#example-create-startup)
  - [Technologies Endpoints](#technologies-endpoints)
    - [Example: Create technology](#example-create-technology)
  - [HTTP Status Codes](#http-status-codes)
- [Postman Testing](#postman-testing)
  - [Collection Overview](#collection-overview)
  - [Postman Collection](#postman-collection)
  - [Import](#import)
  - [Documented Test Cases](#documented-test-cases)
    - [Startups](#startups)
    - [Technologies](#technologies)
  - [Testing Examples](#testing-examples)
    - [Successful Startup Creation](#successful-startup-creation)
    - [Error Validation](#error-validation)
    - [Resource Listing](#resource-listing)
- [Data Model](#data-model)
  - [Table: startups](#table-startups)
  - [Table: technologies](#table-technologies)
- [Useful Commands](#useful-commands)
  - [Docker Compose](#docker-compose)
  - [Database](#database)
- [System Features](#system-features)
  - [Backend](#backend-1)
  - [Frontend](#frontend-1)
- [Troubleshooting](#troubleshooting)
  - [Services not responding](#services-not-responding)
  - [Database connection error](#database-connection-error)
  - [Frontend does not load data](#frontend-does-not-load-data)
  - [Port already in use](#port-already-in-use)
  - [Clean and rebuild](#clean-and-rebuild)
- [Author](#author)
- [License](#license)

---

## Description

Startup and emerging technologies management system built with a decoupled microservices architecture. Each CRUD operation is implemented as an independent microservice, exposed through an Nginx API Gateway, with a responsive React frontend.

| Component | Description |
|----------|-------------|
| Domains | Startups and Emerging Technologies |
| Microservices | 8 independent services (4 per domain) |
| Gateway | Nginx as reverse proxy |
| Database | PostgreSQL 15 |
| Frontend | React with light/dark theme |

---

## System Architecture

```
                              +------------------+
                              |  Frontend React  |
                              |  localhost:3000  |
                              +--------+---------+
                                       |
                                       | HTTP
                                       v
                              +------------------+
                              |   API Gateway    |
                              |     (Nginx)      |
                              |  localhost:8080  |
                              +--------+---------+
                                       |
           +----------+----------+-----+-----+----------+----------+
           |          |          |           |          |          |
           v          v          v           v          v          v
      +--------+ +--------+ +--------+  +--------+ +--------+ +--------+
      | Create | | Read   | | Update |  | Delete | | Create | | Read   | ...
      |Startup | |Startup | |Startup |  |Startup | | Tech   | | Tech   |
      +--------+ +--------+ +--------+  +--------+ +--------+ +--------+
           |          |          |           |          |          |
           +----------+----------+-----------+----------+----------+
                                       |
                              +--------v---------+
                              |   PostgreSQL     |
                              |  localhost:5432  |
                              +------------------+
```

### Microservices

**Startups:**
- create-startup (port 3001)
- read-startup (port 3002)
- update-startup (port 3003)
- delete-startup (port 3004)

**Technologies:**
- create-technology (port 3005)
- read-technology (port 3006)
- update-technology (port 3007)
- delete-technology (port 3008)

---

## Project Structure

```
reto1/
├── gateway/
│   ├── nginx.conf              # Nginx configuration for local development
│   ├── nginx.render.conf       # Nginx configuration for production
│   └── Dockerfile              # API Gateway image
│
├── services/
│   ├── startups/
│   │   ├── create/
│   │   │   ├── index.js        # CREATE microservice
│   │   │   ├── package.json
│   │   │   └── Dockerfile
│   │   ├── read/
│   │   │   ├── index.js        # READ microservice
│   │   │   ├── package.json
│   │   │   └── Dockerfile
│   │   ├── update/
│   │   │   ├── index.js        # UPDATE microservice
│   │   │   ├── package.json
│   │   │   └── Dockerfile
│   │   └── delete/
│   │       ├── index.js        # DELETE microservice
│   │       ├── package.json
│   │       └── Dockerfile
│   │
│   └── technologies/
│       ├── create/
│       │   ├── index.js        # CREATE microservice
│       │   ├── package.json
│       │   └── Dockerfile
│       ├── read/
│       │   ├── index.js        # READ microservice
│       │   ├── package.json
│       │   └── Dockerfile
│       ├── update/
│       │   ├── index.js        # UPDATE microservice
│       │   ├── package.json
│       │   └── Dockerfile
│       └── delete/
│           ├── index.js        # DELETE microservice
│           ├── package.json
│           └── Dockerfile
│
├── frontend/
│   ├── public/
│   │   ├── index.html          # Main HTML
│   │   ├── manifest.json       # PWA manifest
│   │   └── robots.txt          # SEO robots
│   │
│   ├── src/
│   │   ├── components/
│   │   │   ├── common/
│   │   │   │   ├── Alert.js            # Alert component
│   │   │   │   ├── ConfirmDialog.js    # Confirmation dialog
│   │   │   │   ├── FormInput.js        # Reusable input
│   │   │   │   ├── FormSelect.js       # Reusable select
│   │   │   │   ├── Modal.js            # Generic modal
│   │   │   │   └── Spinner.js          # Loading indicator
│   │   │   │
│   │   │   ├── Startups/
│   │   │   │   ├── StartupForm.js      # Create/edit startup form
│   │   │   │   ├── StartupsFilters.js  # Search filters
│   │   │   │   ├── StartupsList.js     # Main container
│   │   │   │   └── StartupsTable.js    # Startups table
│   │   │   │
│   │   │   └── Technologies/
│   │   │       ├── TechnologiesFilters.js  # Search filters
│   │   │       ├── TechnologiesGrid.js     # Technologies grid
│   │   │       ├── TechnologiesList.js     # Main container
│   │   │       └── TechnologyForm.js       # Create/edit form
│   │   │
│   │   ├── hooks/
│   │   │   ├── useApiCall.js   # Hook for API calls
│   │   │   ├── useForm.js      # Hook for form handling
│   │   │   └── useTheme.js     # Hook for light/dark theme
│   │   │
│   │   ├── services/
│   │   │   └── api.js          # Axios client and API services
│   │   │
│   │   ├── utils/
│   │   │   ├── formatters.js   # Formatting functions
│   │   │   └── validators.js   # Form validations
│   │   │
│   │   ├── App.js              # Main component
│   │   ├── App.css             # Global styles and themes
│   │   ├── index.js            # Entry point
│   │   └── index.css           # Base styles
│   │
│   ├── .env                    # Environment variables (create from .env.example)
│   ├── .env.example            # Environment variables template
│   ├── .gitignore              # Git ignored files
│   ├── package.json            # Frontend dependencies
│   ├── package-lock.json       # Dependency lock
│   ├── nginx.conf              # Nginx configuration for frontend
│   ├── Dockerfile              # Frontend image
│   └── README.md               # Frontend documentation
│
├── db/
│   └── init.sql                # DB initialization script
│
├── capturas/
│   ├── postman/                # Postman test screenshots
│   └── responsive/             # Responsive design screenshots
│
├── docker-compose.yml          # Container orchestration
├── Microservices_CRUD_Collection.postman.json  # Postman collection
└── README.md                   # This file
```

---

## Technology Stack

### Backend

| Technology | Version | Usage |
|-----------|---------|------|
| Node.js | 18 | Runtime |
| Express.js | 4.x | HTTP framework |
| PostgreSQL | 15 | Database |
| pg | 8.x | PostgreSQL client |
| Nginx | Alpine | API Gateway |
| Docker | 20.10+ | Containers |
| Docker Compose | 2.x | Orchestration |

### Frontend

| Technology | Version | Usage |
|-----------|---------|------|
| React | 19 | UI framework |
| React Router DOM | 7.x | Routing |
| Axios | 1.x | HTTP client |
| CSS Variables | - | Light/dark themes |

---

## Prerequisites

- Docker Desktop 20.10 or higher
- Docker Compose (included with Docker Desktop)
- Git

### Installation verification

```bash
docker --version
docker-compose --version
```

---

## Installation and Execution

### Step 1: Clone the repository

```bash
git clone <repository-url>
cd reto1
```

### Step 2: Start services with Docker Compose

```bash
docker-compose up --build
```

This command:
- Builds the images for the 8 microservices
- Builds the API Gateway image
- Builds the frontend image
- Creates the PostgreSQL database
- Runs the init.sql script to create tables and insert test data
- Brings up all containers on the Docker network

Estimated time: 3-5 minutes on the first build.

### Step 3: Verify that services are running

```bash
docker-compose ps
```

It should show 11 active containers:
- 1 frontend
- 1 gateway
- 8 microservices
- 1 postgres

### Step 4: Access the application

| Service | URL |
|--------|-----|
| Frontend | http://localhost:3000 |
| API Gateway | http://localhost:8080 |
| PostgreSQL | localhost:5432 |

---

## Environment Variables

The docker-compose.yml file uses the following variables with default values:

```env
DB_HOST=postgres
DB_PORT=5432
DB_NAME=reto_db
DB_USER=postgres
DB_PASSWORD=postgres123
```

---

## API Routes

### Base URL

```
http://localhost:8080/v1/api
```

### Startups Endpoints

| Method | Endpoint | Description |
|-------|----------|-------------|
| POST | /v1/api/startups/create | Create startup |
| GET | /v1/api/startups/read | List startups |
| GET | /v1/api/startups/read/:id | Get startup by ID |
| GET | /v1/api/startups/read?name=X | Filter by name |
| GET | /v1/api/startups/read?category=X | Filter by category |
| PUT | /v1/api/startups/update/:id | Update startup |
| DELETE | /v1/api/startups/delete/:id | Delete startup |

#### Example: Create startup

```http
POST /v1/api/startups/create
Content-Type: application/json

{
  "name": "TechVision AI",
  "founded_at": "2023-01-15",
  "location": "San Francisco, CA",
  "category": "Artificial Intelligence",
  "funding_amount": 5000000
}
```

Response (201 Created):

```json
{
  "id": 1,
  "name": "TechVision AI",
  "founded_at": "2023-01-15",
  "location": "San Francisco, CA",
  "category": "Artificial Intelligence",
  "funding_amount": 5000000,
  "created_at": "2025-10-06T10:30:00Z",
  "updated_at": "2025-10-06T10:30:00Z"
}
```

### Technologies Endpoints

| Method | Endpoint | Description |
|-------|----------|-------------|
| POST | /v1/api/technologies/create | Create technology |
| GET | /v1/api/technologies/read | List technologies |
| GET | /v1/api/technologies/read/:id | Get technology by ID |
| GET | /v1/api/technologies/read?sector=X | Filter by sector |
| GET | /v1/api/technologies/read?adoption_level=X | Filter by adoption level |
| PUT | /v1/api/technologies/update/:id | Update technology |
| DELETE | /v1/api/technologies/delete/:id | Delete technology |

#### Example: Create technology

```http
POST /v1/api/technologies/create
Content-Type: application/json

{
  "name": "Quantum Computing",
  "sector": "Computing",
  "description": "Advanced computing using quantum mechanics",
  "adoption_level": "emerging"
}
```

Valid values for `adoption_level`: emerging, growing, mature, declining

### HTTP Status Codes

| Code | Description |
|--------|-------------|
| 200 | OK - Successful operation |
| 201 | Created - Resource created |
| 204 | No Content - Successful deletion |
| 400 | Bad Request - Invalid data |
| 404 | Not Found - Resource not found |
| 500 | Internal Server Error |

---

## Postman Testing

### Collection Overview

![Postman Collection](./capturas/postman/01-postman-collection-vista-general.png)

### Postman Collection

The `Microservices_CRUD_Collection.postman.json` file includes 18 pre-configured requests to test all endpoints.

### Import

1. Open Postman
2. File > Import
3. Select `Microservices_CRUD_Collection.postman.json`
4. Set the `baseUrl` variable to `http://localhost:8080/v1/api`

### Documented Test Cases

Screenshots in `/capturas/postman/` document the following tests:

#### Startups

| Test | Endpoint | Method | Expected Status | Screenshot |
|--------|----------|--------|-----------------|---------|
| 01 | Collection | - | Import OK | ![](./capturas/postman/01-postman-collection-vista-general.png) |
| 02 | /startups/create | POST | 201 | ![](./capturas/postman/02-startup-create-success.png) |
| 03 | /startups/create | POST | 400 (validation) | ![](./capturas/postman/03-startup-create-fail.png) |
| 04 | /startups/read | GET | 200 | ![](./capturas/postman/04-startup-read-all.png) |
| 05 | /startups/read?category=X | GET | 200 | ![](./capturas/postman/05-startup-read-filtered.png) |
| 06 | /startups/read/:id | GET | 200 | ![](./capturas/postman/06-startup-read-by-id.png) |
| 07 | /startups/update/:id | PUT | 200 | ![](./capturas/postman/07-startup-update-success.png) |
| 08 | /startups/delete/:id | DELETE | 200 | ![](./capturas/postman/08-startup-delete-200.png) |
| 09 | /startups/delete/99999 | DELETE | 404 | ![](./capturas/postman/09-startup-delete-404.png) |

#### Technologies

| Test | Endpoint | Method | Expected Status | Screenshot |
|--------|----------|--------|-----------------|---------|
| 10 | /technologies/create | POST | 201 | ![](./capturas/postman/10-technology-create-success.png) |
| 11 | /technologies/read | GET | 200 | ![](./capturas/postman/11-technology-read-filtered.png) |
| 12 | /technologies/update/:id | PUT | 200 | ![](./capturas/postman/12-technology-update-success.png) |
| 13 | /technologies/delete/:id | DELETE | 200 | ![](./capturas/postman/13-technology-delete-200.png) |

### Testing Examples

#### Successful Startup Creation

![Startup Create Success](./capturas/postman/02-startup-create-success.png)

#### Error Validation

![Startup Create Fail](./capturas/postman/03-startup-create-fail.png)

#### Resource Listing

![Startup Read All](./capturas/postman/04-startup-read-all.png)

---

## Data Model

### Table: startups

| Field | Type | Constraints |
|-------|------|-------------|
| id | SERIAL | PRIMARY KEY |
| name | VARCHAR(255) | NOT NULL |
| founded_at | DATE | NOT NULL |
| location | VARCHAR(255) | NOT NULL |
| category | VARCHAR(100) | NOT NULL |
| funding_amount | DECIMAL(15,2) | DEFAULT 0 |
| created_at | TIMESTAMP | DEFAULT CURRENT_TIMESTAMP |
| updated_at | TIMESTAMP | DEFAULT CURRENT_TIMESTAMP |

### Table: technologies

| Field | Type | Constraints |
|-------|------|-------------|
| id | SERIAL | PRIMARY KEY |
| name | VARCHAR(255) | NOT NULL |
| sector | VARCHAR(100) | NOT NULL |
| description | TEXT | - |
| adoption_level | VARCHAR(50) | NOT NULL, CHECK (emerging, growing, mature, declining) |
| created_at | TIMESTAMP | DEFAULT CURRENT_TIMESTAMP |
| updated_at | TIMESTAMP | DEFAULT CURRENT_TIMESTAMP |

---

## Useful Commands

### Docker Compose

```bash
# Start services in the background
docker-compose up -d

# Start with rebuild
docker-compose up --build

# Stop services
docker-compose down

# View logs for all services
docker-compose logs -f

# View logs for a specific service
docker-compose logs -f gateway

# View container status
docker-compose ps

# Restart a service
docker-compose restart gateway

# Remove volumes (includes DB data)
docker-compose down -v
```

### Database

```bash
# Connect to PostgreSQL
docker exec -it reto_db psql -U postgres -d reto_db

# List tables
\dt

# View startups data
SELECT * FROM startups;

# View technologies data
SELECT * FROM technologies;

# Exit
\q
```

---

## System Features

### Backend

- 8 independent microservices with single responsibility
- Nginx API Gateway with route versioning (/v1/api)
- PostgreSQL with normalized tables and indexes
- Input validation on all endpoints
- Consistent error handling with appropriate HTTP codes
- /health endpoints for monitoring
- CORS configured
- Docker Compose for orchestration

### Frontend

- Full CRUD for both domains
- Dynamic filters (name, category, sector, adoption level)
- Client-side validation before submitting
- Descriptive error messages
- Loading indicators during requests
- Light/dark theme with persistence in localStorage
- Responsive design
- Reusable components
- Custom hooks (useTheme, useForm, useApiCall)
- Routing with React Router

---

## Troubleshooting

### Services not responding

```bash
# Check container status
docker-compose ps

# Check logs to identify errors
docker-compose logs

# Restart all services
docker-compose down
docker-compose up --build
```

### Database connection error

```bash
# Verify PostgreSQL is running
docker-compose ps postgres

# View PostgreSQL logs
docker-compose logs postgres

# Wait for healthcheck to pass
docker-compose up -d
# Wait 15 seconds and verify
docker-compose ps
```

### Frontend does not load data

1. Verify the API Gateway is running: http://localhost:8080/health
2. Check gateway logs: `docker-compose logs gateway`
3. Verify microservices respond: `docker-compose logs read-startup`

### Port already in use

```bash
# See which process is using the port (Windows PowerShell)
netstat -ano | findstr :3000

# Stop containers and free ports
docker-compose down
```

### Clean and rebuild

```bash
# Remove containers, networks, and volumes
docker-compose down -v

# Remove project images
docker rmi $(docker images "reto1*" -q)

# Rebuild from scratch
docker-compose up --build
```

---

## Author

Carlos Elias Linares Ojeda

---

## License

This project was developed as part of a technical challenge.

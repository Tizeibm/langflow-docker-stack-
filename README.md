# 🧠 Infrastructure as Code : Déploiement Sécurisé de Langflow

## 📋 Description du Projet
Ce dépôt contient l'architecture as code (IaC) utilisée pour déployer et orchestrer **Langflow** (interface visuelle de création d'agents IA et pipelines LLM) via **Docker Compose**. 

L'objectif de ce projet est de démontrer la mise en production d'une stack IA open-source complexe dans un environnement isolé, hautement sécurisé et facilement reproductible. 

> **Note de sécurité :** Il s'agit d'un modèle (template) d'infrastructure. Les variables d'environnement sensibles, les mots de passe et les configurations réseaux spécifiques de production ont été anonymisés via un fichier `.env.example`.

## 🏗️ Architecture Technique
L'ensemble de la stack repose sur des conteneurs Docker communiquant via un réseau virtuel interne (`app-net`), garantissant l'isolation des processus. Le service Telegram Bridge a été délibérément exclu de ce dépôt pour se concentrer sur le cœur de l'infrastructure IA.

*   **Orchestration :** Docker Compose
*   **Application principale :** Langflow
*   **Bases de données relationnelles & vectorielles :** PostgreSQL et PgVector (dédié à l'ingestion de vecteurs)
*   **Bases de données NoSQL :** MongoDB (Atlas Local)
*   **Inférence LLM Locale :** Ollama

## ⚙️ Prérequis
Pour lancer cette infrastructure en local ou sur un serveur (environnement Linux recommandé) :
*   [Docker](https://docs.docker.com/get-docker/) (v20.10+)
*   [Docker Compose](https://docs.docker.com/compose/install/) (v2.x)

## 🚀 Instructions de Déploiement

### 1. Cloner le dépôt :
```bash
git clone [https://github.com/](https://github.com/)[Ton-Nom-Utilisateur]/langflow-docker-stack.git
cd langflow-docker-stack
```

# 🧠 Infrastructure as Code : Déploiement Sécurisé de Langflow

## 📋 Description du Projet
Ce dépôt contient l'architecture as code (IaC) utilisée pour déployer et orchestrer **Langflow** (interface visuelle de création d'agents IA et pipelines LLM) via **Docker Compose**. 

L'objectif de ce projet est de démontrer la mise en production d'une stack IA open-source complexe dans un environnement isolé, hautement sécurisé et facilement reproductible. 

> **Note de sécurité :** Il s'agit d'un modèle (template) d'infrastructure. Les variables d'environnement sensibles, les mots de passe et les configurations réseaux spécifiques de production ont été anonymisés via un fichier `.env.example`.

## 🏗️ Architecture Technique
L'ensemble de la stack repose sur des conteneurs Docker communiquant via un réseau virtuel interne (`app-net`), garantissant l'isolation des processus. Le service Telegram Bridge a été délibérément exclu de ce dépôt pour se concentrer sur le cœur de l'infrastructure IA.

*   **Orchestration :** Docker Compose
*   **Application principale :** Langflow
*   **Bases de données relationnelles & vectorielles :** PostgreSQL et PgVector (dédié à l'ingestion de vecteurs)
*   **Bases de données NoSQL :** MongoDB (Atlas Local)
*   **Inférence LLM Locale :** Ollama

## ⚙️ Prérequis
Pour lancer cette infrastructure en local ou sur un serveur (environnement Linux recommandé) :
*   [Docker](https://docs.docker.com/get-docker/) (v20.10+)
*   [Docker Compose](https://docs.docker.com/compose/install/) (v2.x)

## 🚀 Instructions de Déploiement

### 1. Cloner le dépôt :
```bash
git clone [https://github.com/](https://github.com/)[Ton-Nom-Utilisateur]/langflow-docker-stack.git
cd langflow-docker-stack 
```
### 2. Configuration de l'environnement:
Copier le fichier d'exemple et remplir les variables nécessaires avec vos propres identifiants sécurisés.
```bash
cp .env.example .env
```
Assurez-vous de définir un **LANGFLOW_SUPERUSER** robuste lors de l'initialisation pour sécuriser l'accès, et de vérifier les identifiants des différentes bases de données.

### 3. Démarrage des services :
Lancer l'ensemble de la stack en arrière-plan.
```bash
docker compose up -d
```

### 4. Vérification du statut et des logs :
```bash
docker compose ps
docker compose logs -f langflow
```
Langflow sera accessible localement (ou via votre reverse proxy) sur le port 7860.

## 🔧 Défis Techniques & Configurations Spécifiques
Lors de la conception de cette architecture, plusieurs points critiques d'intégration ont été gérés pour garantir la stabilité du déploiement :
*	**Communication Inter-conteneurs** : Résolution des problèmes de connectivité en exploitant le DNS natif de Docker (ex: cibler le service **postgres** ou **ollama** directement via le réseau **app-net**).
*	Sécurité SSRF (Server-Side Request Forgery) : Configuration stricte de la variable **LANGFLOW_SSRF_ALLOWED_HOSTS** pour n'autoriser que les connexions internes légitimes vers les bases de données et modèles locaux.
*	**Ingestion Vectorielle **: Paramétrage d'un conteneur **pgvector** dédié, indispensable au fonctionnement des capacités de mémorisation IA (RAG) de Langflow.

## 🛡️ DevSecOps & Hardening
Ce déploiement intègre des pratiques de sécurité avancées au niveau des conteneurs :
*	**Utilisateurs Non-Root :** Exécution des services (Postgres, Langflow, etc.) avec des UID/GID restreints (ex: **user: "999:999"**).
*	**Restriction des Privilèges :** Utilisation systématique de **security_opt: no-new-privileges:true**.
*	**Capabilities Linux Minimales :** Application du principe de moindre privilège avec **cap_drop: ALL** et ajout chirurgical des seules permissions strictement nécessaires (**CHOWN, SETUID, NET_BIND_SERVICE**).

## 👨‍💻 Auteur
Tize Ibrahim Ahmed - Futur Ingénieur Cloud & DevOps
*	LinkedIn : https://www.linkedin.com/in/tize-ibrahim-ahmed-1525b6334 

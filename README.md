# 🏨 Projet Gestion Hôtelière – Architecture Cloud-Native

En raison de la volumétrie importante du projet (images Docker, manifests Kubernetes, scripts d’initialisation et fichiers associés), le dépôt complet du code source n’a pas pu être hébergé directement sur la plateforme du CNAM.

L’intégralité du code source de l’API Backend (FastAPI), ainsi que les fichiers nécessaires au déploiement (Dockerfiles, manifests Kubernetes, scripts SQL), est disponible via un espace de stockage externe sécurisé.

🔗 Lien de téléchargement du code source (Proton Drive) :

👉 https://drive.proton.me/urls/M37SRFVB1C#NzeOEGh1bUQF

Ce lien permet :

- d’accéder au code source complet du projet,

- de reproduire l’environnement Docker et Kubernetes,

- de vérifier le fonctionnement de l’API et de l’architecture Cloud-Native mise en place.

## 📌 Présentation

Ce projet est une application web **full‑stack de gestion hôtelière** permettant l’administration des hôtels, chambres, clients et réservations.

Initialement développée avec **Docker Compose**, l’application a ensuite été **migrée vers une architecture Cloud‑Native orchestrée par Kubernetes (K3s)** sur un cluster multi‑nœuds, afin de démontrer :

* la **haute disponibilité**,
* la **scalabilité horizontale**,
* les mécanismes d’**auto‑healing** de Kubernetes,
* une approche proche des **standards industriels**.

---

## 👥 Équipe Projet

* **Savoglou Enzo**
* **Ballatore Fabien**
* **Lirzin Erwan**
* **Bezara Jonhatan**
* **Petit Willian**

---

## 🚀 Fonctionnalités

### 🏨 Gestion des hôtels

* Création, consultation et suppression des hôtels

### 🚪 Gestion des chambres

* CRUD complet
* Suivi dynamique de l’état : `libre`, `occupée`, `maintenance`

### 👤 Gestion des clients

* Centralisation et consultation des informations clients

### 📅 Gestion des réservations

* Création de réservations liées aux clients et chambres
* Suppression des réservations

---

## 🛠️ Stack Technique

### 🧠 Backend

* **FastAPI** (Python 3.9+)
* API REST performante et auto‑documentée
* SQLAlchemy & Pydantic

### 🎨 Frontend

* HTML5 / CSS3 / JavaScript
* Interface simple et réactive
* Servi via **Nginx / Apache**

### 🗄️ Base de données

* **MariaDB 10.6**
* Modèle relationnel

### ☁️ Infrastructure

* **Docker** : conteneurisation
* **Kubernetes K3s** : orchestration

---

## 📂 Structure du Projet

```bash
.
├── backend/            # API FastAPI
│   └── Dockerfile      # Image : hotel-backend:v2
├── frontend/           # Interface Web
│   └── Dockerfile      # Image : hotel-frontend:v2
├── BDD/                # Base de données
│   └── init.sql        # Initialisation des tables et données
├── Kubernetes/                # Manifestes Kubernetes
│   └── hotel-deploy.yaml, kubernetes-dashboard.yam # Deployments & Services
└── docker-compose.yaml # Environnement de développement local
```

---
## 📖 Documentation de l’API

L’API est auto‑documentée grâce à **Swagger UI**.

📍 **Accès Swagger** :
👉 `http://192.168.1.31:30001/docs`

---

## 🔌 Endpoints API

### 🏨 Hôtels – `/hotels`

* `POST /` : Créer un hôtel
* `GET /` : Lister les hôtels
* `GET /{hotel_id}` : Détails d’un hôtel
* `DELETE /{hotel_id}` : Supprimer un hôtel

### 🚪 Chambres – `/chambres`

* `POST /` : Créer une chambre
* `GET /{hotel_id}` : Chambres d’un hôtel
* `PUT /{chambre_id}` : Mettre à jour une chambre
* `GET /{chambre_id}` : État d’une chambre

### 👤 Clients – `/clients`

* `POST /` : Créer un client
* `GET /` : Lister les clients
* `GET /{client_id}` : Détails d’un client

### 📅 Réservations – `/reservations`

* `POST /` : Créer une réservation
* `GET /` : Lister les réservations
* `DELETE /{reservation_id}` : Supprimer une réservation

---

## ☸️ Orchestration Kubernetes (K3s)

Le projet est déployé sur un **cluster Kubernetes K3s composé de 3 machines virtuelles Debian**, interconnectées via un réseau privé.

### 🏗️ Architecture du Cluster

| Rôle   | Nom          | Adresse IP   | Description                          |
| ------ | ------------ | ------------ | ------------------------------------ |
| Master | docker-host1 | 192.168.1.31 | API Server, Scheduler, Control Plane |
| Worker | docker-host2 | 192.168.1.32 | Pods applicatifs                     |
| Worker | docker-host3 | 192.168.1.33 | Pods applicatifs                     |

---

### 📦 Ressources Kubernetes Déployées

* **Deployments** :

  * Backend
  * Frontend
  * MariaDB

* **Services** :

  * `Frontend` → **NodePort : 30080**
  * `Backend` → **NodePort : 30001**
  * `MariaDB` → **ClusterIP : 3306**

---

## 🖥️ Administration & Monitoring

### 📊 Dashboard Kubernetes

Accès sécurisé via proxy :

```bash
sudo kubectl proxy --address='0.0.0.0' --accept-hosts='^*$'
```

Tunnel SSH :

```bash
ssh -L 8001:localhost:8001 user@192.168.1.31
```

Token d’accès :

```bash
kubectl -n kubernetes-dashboard create token admin-user
```

---

## 💾 Persistance des Données

Actuellement :

* Initialisation via script SQL (`init.sql`)
* Stockage persistant pour MariaDB **Persistent Volume Claims (PVC)**
* Conservation des données après redémarrage des pods ou VMs

---

## 📺 Démo Vidéo

Des tests de panne ont été réalisés afin de valider la robustesse du cluster.

* Détection automatique de la perte du pod
* Replanification automatique sur `docker-host2`
* Temps de reprise < **30 secondes**
* **Aucune interruption de service visible**

Lien : [Regarder la vidéo sur YouTube](https://youtu.be/AUPaTVntRBY)

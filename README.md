# Guide Complet pour Apprendre Docker

Bienvenue dans ce cours complet sur Docker. Ce guide vous apprendra tout ce qu'il faut savoir, de l'installation à des cas d'utilisation avancés.

## Table des Matières

1. [Introduction à Docker](#introduction-à-docker)
2. [Installation de Docker](#installation-de-docker)
3. [Concepts Fondamentaux](#concepts-fondamentaux)
4. [Gestion des Conteneurs](#gestion-des-conteneurs)
5. [Création et Gestion d’Images Docker](#création-et-gestion-dimages-docker)
6. [Volumes et Réseaux](#volumes-et-réseaux)
7. [Docker Compose](#docker-compose)
8. [Optimisation et Bonnes Pratiques](#optimisation-et-bonnes-pratiques)
9. [Sécurité avec Docker](#sécurité-avec-docker)
10. [Cas d’Utilisation Avancés](#cas-dutilisation-avancés)

---

## Introduction à Docker

Docker est une plateforme de conteneurisation qui permet de créer, déployer et exécuter des applications dans des environnements isolés appelés conteneurs. Ces conteneurs offrent :

- Une portabilité : "Déployez partout où Docker est disponible."
- Une cohérence : "Le code fonctionne de manière identique en local et en production."
- Une efficacité : "Partagez les ressources système sans surcharger."

### Pourquoi Docker ?

- Simplification du déploiement
- Isolation des environnements
- Collaboration accrue entre équipes de développement et d’opérations

---

## Installation de Docker

### Installation sur Linux

1. **Mettez à jour votre système** :
   ```bash
   sudo apt update && sudo apt upgrade
   ```
2. **Ajoutez le dépôt officiel Docker** :
   ```bash
   sudo apt install -y ca-certificates curl gnupg
   curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /usr/share/keyrings/docker-archive-keyring.gpg
   echo "deb [arch=$(dpkg --print-architecture) signed-by=/usr/share/keyrings/docker-archive-keyring.gpg] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
   ```
3. **Installez Docker** :
   ```bash
   sudo apt update
   sudo apt install -y docker-ce docker-ce-cli containerd.io
   ```
4. **Ajoutez votre utilisateur au groupe Docker** :
   ```bash
   sudo usermod -aG docker $USER
   ```
5. **Vérifiez l'installation** :
   ```bash
   docker --version
   ```

### Installation sur Windows / macOS

Téléchargez et installez [Docker Desktop](https://www.docker.com/products/docker-desktop). Suivez les instructions de l'interface graphique pour terminer l'installation.

---

## Concepts Fondamentaux

### Conteneur
Un conteneur est une instance d’une image Docker. C’est un environnement isolé pour exécuter des applications.

### Image
Une image est un modèle immuable utilisé pour créer des conteneurs. Les images sont créées à partir d’instructions dans un fichier `Dockerfile`.

### Docker Engine
Le moteur Docker exécute les conteneurs et gère les ressources système nécessaires.

### Registre
Un registre est un service où les images sont stockées, comme Docker Hub.

---

## Gestion des Conteneurs

### Lancer un Conteneur
```bash
docker run -d --name mon_conteneur nginx
```
- `-d` : Détaché (en arrière-plan)
- `--name` : Nom personnalisé du conteneur

### Lister les Conteneurs
```bash
docker ps
```

### Arrêter un Conteneur
```bash
docker stop mon_conteneur
```

### Supprimer un Conteneur
```bash
docker rm mon_conteneur
```

### Inspecter un Conteneur
```bash
docker inspect mon_conteneur
```

---

## Création et Gestion d’Images Docker

### Créer une Image
1. **Écrivez un `Dockerfile`** :
   ```dockerfile
   FROM ubuntu:20.04
   RUN apt-get update && apt-get install -y nginx
   CMD ["nginx", "-g", "daemon off;"]
   ```
2. **Construisez l’image** :
   ```bash
   docker build -t mon_image:1.0 .
   ```

### Lister les Images
```bash
docker images
```

### Supprimer une Image
```bash
docker rmi mon_image:1.0
```

---

## Volumes et Réseaux

### Gestion des Volumes
Les volumes sont utilisés pour persister les données.

- **Créer un volume** :
  ```bash
  docker volume create mon_volume
  ```
- **Monter un volume dans un conteneur** :
  ```bash
  docker run -d -v mon_volume:/data nginx
  ```

### Gestion des Réseaux

- **Créer un réseau** :
  ```bash
  docker network create mon_reseau
  ```
- **Connecter un conteneur à un réseau** :
  ```bash
  docker network connect mon_reseau mon_conteneur
  ```

---

## Docker Compose

Docker Compose permet de gérer plusieurs conteneurs avec un fichier YAML.

### Exemple `docker-compose.yml`
```yaml
version: '3.8'
services:
  web:
    image: nginx
    ports:
      - "8080:80"
  db:
    image: mysql
    environment:
      MYSQL_ROOT_PASSWORD: exemple
```

### Commandes Utiles

- **Démarrer les services** :
  ```bash
  docker-compose up -d
  ```
- **Arrêter les services** :
  ```bash
  docker-compose down
  ```

---

## Optimisation et Bonnes Pratiques

- Utilisez des images officielles et légères comme `alpine`.
- Nettoyez régulièrement vos conteneurs et images inutilisés :
  ```bash
  docker system prune
  ```
- Documentez vos `Dockerfile` et `docker-compose.yml`.

---

## Sécurité avec Docker

- Limitez les permissions des conteneurs avec les options comme `--read-only`.
- Scannez vos images avec des outils comme `docker scan` ou `Trivy`.
- Configurez des pare-feu pour protéger vos réseaux Docker.

---

## Cas d’Utilisation Avancés

### Multi-Stage Builds
Optimisez vos images en utilisant des builds multi-étapes.

```dockerfile
FROM golang:1.18 as builder
WORKDIR /app
COPY . .
RUN go build -o main .

FROM alpine:latest
WORKDIR /root/
COPY --from=builder /app/main .
CMD ["./main"]
```

### Déploiement avec Swarm

Docker Swarm permet de déployer des applications conteneurisées à grande échelle.

1. **Initier un Swarm** :
   ```bash
   docker swarm init
   ```
2. **Déployer un service** :
   ```bash
   docker service create --replicas 3 -p 80:80 nginx
   ```

---

Félicitations, vous avez terminé ce guide ! 🎉

Pour approfondir, visitez la [documentation officielle Docker](https://docs.docker.com).


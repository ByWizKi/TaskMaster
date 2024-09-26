# Configuration environnement Docker

## Prérequis

Avant de commencer, assurez-vous d’avoir les outils suivants installés sur votre machine :

1. **Docker** et **Docker Compose**
   - Docker Desktop (pour macOS et Windows) : [Télécharger ici](https://www.docker.com/products/docker-desktop)
   - Sur Ubuntu/Debian :
     ```bash
     sudo apt update
     sudo apt install docker.io docker-compose
     ```

2. **Git** pour cloner le dépôt du projet
   - [Installer Git](https://git-scm.com/)

---

## Étapes de configuration

### 1. **Cloner le dépôt du projet**

Commencez par cloner le dépôt Git du projet **TaskMaster**. Ouvrez un terminal et exécutez la commande suivante :

```bash
git clone https://github.com/votre-repo/taskmaster.git
cd taskmaster
```

---

### 2. **Configuration avec Docker**

Nous allons utiliser Docker pour gérer l'environnement de développement. Cela vous permet de ne pas avoir à installer manuellement les dépendances comme Python ou PostgreSQL.

#### a. **Vérifier la structure du projet**

Après avoir cloné le projet, la structure des dossiers devrait ressembler à ceci :

```plaintext
taskmaster/
│
├── backend/                # Dossier pour le code backend (Django)
│   ├── backend/            # Sous-dossier contenant les fichiers Django (settings.py, wsgi.py, etc.)
│   ├── manage.py           # Fichier principal pour lancer Django
│   ├── requirements.txt    # Fichier des dépendances Python
│   └── Dockerfile          # Dockerfile pour containeriser le backend Django
│
├── docker-compose.yml       # Fichier de configuration Docker Compose
└── other-folders/           # Autres dossiers éventuels (frontend, mobile, etc.)
```

#### b. **Docker Compose : Services configurés**

Le fichier **`docker-compose.yml`** est configuré pour démarrer deux services :
1. **PostgreSQL** : Base de données pour Django.
2. **Django** : L'application Django elle-même.

#### c. **Construire et démarrer les services**

Exécutez la commande suivante à la racine du projet pour construire les images Docker et démarrer les services :

```bash
docker-compose build
docker-compose up
```

Cela démarrera à la fois le backend Django et la base de données PostgreSQL. Vous pouvez accéder à l'application Django sur [http://localhost:8000](http://localhost:8000).

#### d. **Appliquer les migrations**

Une fois que les services sont démarrés, vous devez exécuter les migrations de base de données pour initialiser PostgreSQL avec les bonnes tables :

```bash
docker-compose exec web python manage.py migrate
```

#### e. **Créer un super utilisateur (facultatif)**

Si vous avez besoin d'accéder à l'interface d'administration Django, créez un super utilisateur avec cette commande :

```bash
docker-compose exec web python manage.py createsuperuser
```

---

### 3. **Utilisation de l'application Django**

Une fois les services démarrés avec Docker et les migrations appliquées, vous pouvez utiliser Django comme d'habitude. Voici quelques commandes utiles :

#### a. **Redémarrer les services Docker**

Pour redémarrer tous les services (base de données et backend) après un arrêt ou une modification :

```bash
docker-compose down
docker-compose up
```

#### b. **Exécuter des tests Django**

Pour exécuter les tests unitaires de l'application :

```bash
docker-compose exec web python manage.py test
```

#### c. **Accéder à PostgreSQL**

Si vous souhaitez accéder à la base de données PostgreSQL pour vérifier son état ou exécuter des requêtes manuelles :

```bash
docker-compose exec db psql -U myuser -d taskmaster_db
```

---

### 4. **Dépannage**

Voici quelques conseils en cas de problèmes :

#### a. **Container ne démarre pas**

- **Problème de port en cours d'utilisation** : Si le port 8000 ou 5432 est déjà utilisé, modifiez les ports dans **`docker-compose.yml`**.

- **Supprimer les containers et les images** :
  ```bash
  docker-compose down --volumes
  docker system prune -a
  ```

#### b. **Appliquer des migrations non appliquées**

Si vous voyez des erreurs liées à la base de données, vous pouvez essayer de réappliquer les migrations ou réinitialiser la base de données :

```bash
docker-compose exec web python manage.py migrate
docker-compose exec web python manage.py makemigrations
```

---

### 5. **Documentation des commandes principales**

Voici un récapitulatif des commandes couramment utilisées :

#### a. **Démarrer les services Docker** :

```bash
docker-compose up
```

#### b. **Arrêter les services Docker** :

```bash
docker-compose down
```

#### c. **Exécuter les migrations Django** :

```bash
docker-compose exec web python manage.py migrate
```

#### d. **Créer un super utilisateur Django** :

```bash
docker-compose exec web python manage.py createsuperuser
```

#### e. **Exécuter les tests unitaires** :

```bash
docker-compose exec web python manage.py test
```

---

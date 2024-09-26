# Utiliser une image de base Python
FROM python:3.9-slim

# Définir le répertoire de travail à l'intérieur du container
WORKDIR /app

# Copier les dépendances Python
COPY requirements.txt .

# Installer les dépendances Python
RUN pip install --no-cache-dir -r requirements.txt

# Copier tout le projet dans le container
COPY . .

# Exposer le port 8000 pour Django
EXPOSE 8000

# Commande pour démarrer le serveur Django
CMD ["python", "manage.py", "runserver", "0.0.0.0:8000"]

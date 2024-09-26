# Spécifications techniques - TaskMaster

## 1. Stack technique
- **Backend** : Django avec Django REST Framework pour l'API REST.
- **Frontend mobile** : Flutter pour Android et iOS.
- **Frontend web** : React.js pour l'interface utilisateur web.
- **Base de données** : PostgreSQL pour stocker les utilisateurs, tâches, et statistiques.
- **Notifications** : Firebase Cloud Messaging pour les notifications push sur mobile et web, SendGrid pour les emails.

## 2. Architecture du système
TaskMaster est basé sur une architecture client-serveur avec les composants suivants :
- **Django** gère l'API RESTful pour le backend.
- **React.js** est utilisé pour l'application web.
- **Flutter** pour l'application mobile.
- **PostgreSQL** pour la persistance des données.

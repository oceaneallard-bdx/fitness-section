# Déploiement Render

Ce projet peut tourner en local avec SQLite et en production Render avec PostgreSQL.

## 1. Créer le Web Service

1. Pousser le projet sur GitHub.
2. Dans Render, créer un nouveau **Web Service** connecté au repo GitHub.
3. Configurer :

```bash
Build command: pip install -r requirements.txt
Start command: gunicorn app:app
```

## 2. Créer PostgreSQL

1. Dans Render, créer une base **PostgreSQL**.
2. Copier l’**Internal Database URL**.
3. Dans les variables d’environnement du Web Service, ajouter :

```bash
DATABASE_URL=<Internal Database URL Render>
```

La base PostgreSQL sera vide au premier déploiement. C’est volontaire : aucune migration des données SQLite locales n’est nécessaire.

## 3. Variables d’environnement

Ajouter aussi les variables suivantes dans Render :

```bash
SECRET_KEY=<clé longue et aléatoire>
ADMIN_EMAIL=<email admin>
ADMIN_PASSWORD=<mot de passe admin initial>
ADMIN_NAME=<nom affiché admin>
SMTP_HOST=<serveur SMTP Brevo>
SMTP_PORT=587
SMTP_USER=<utilisateur SMTP Brevo>
SMTP_PASSWORD=<mot de passe SMTP Brevo>
MAIL_FROM=Section Fitness <adresse@email.fr>
```

Le port `587` utilise STARTTLS. Le port `465` reste compatible avec SMTP_SSL.

Au premier accès au site, l'application crée automatiquement les tables et le compte admin initial si la base PostgreSQL est vide. Si `ADMIN_EMAIL` et `ADMIN_PASSWORD` ne sont pas définis, les identifiants de secours sont :

```text
admin@fitness.local / admin123
```

## 4. Usage local

Sans `DATABASE_URL`, l’application continue à utiliser SQLite :

```bash
python3 app.py
```

En production Render, l’application utilise :

```bash
gunicorn app:app
```

# 1-1 Pour quelle raison est-il préférable d'exécuter le conteneur avec un indicateur -e pour fournir les variables d'environnement plutôt que de les placer directement dans le Dockerfile ?

Il est préférable d'utiliser l'indicateur -e au moment de l'exécution pour des raisons de sécurité et de flexibilité :
- Sécurité : Éviter d'exposer les mots de passe et identifiants en clair
- Flexibilité : Permettre de réutiliser la même image Docker sur différents environnements

# Pourquoi avons-nous besoin d'un volume à attacher à notre conteneur postgres ?
Par défaut, les données écrites à l'intérieur d'un conteneur Docker sont stockées dans sa couche inscriptible éphémère. Si le conteneur est supprimé, toutes les données de la base de données sont définitivement perdues. 
L'utilisation d'un volume Docker permet de persister ces données sur le disque de la machine hôte.

# Documentation des éléments essentiels du conteneur de base de données :

Le Dockerfile :
FROM postgres:17.2-alpine
COPY ./init/ /docker-entrypoint-initdb.d/

**Les commandes principales :**

Construction de l'image :
docker build -t mon-postgres .

Création du réseau :
docker network create app-network

Lancement du conteneur avec volume et variables d'environnement :
docker run --name mon-postgres-container --net=app-network -v pgdata:/var/lib/postgresql/data -e POSTGRES_DB=db -e POSTGRES_USER=usr -e POSTGRES_PASSWORD=pwd -d mon-postgres

Lancement d'Adminer :
docker run -p "8090:8080" --net=app-network --name=adminer -d adminer

# Pourquoi avons-nous besoin d'une construction en plusieurs étapes (multi-stage build) ?
Une construction en plusieurs étapes permet de séparer l'environnement de compilation de l'environnement d'exécution final. Cela permet d'obtenir une image finale beaucoup plus légère, plus sécurisée et de s'affranchir de l'installation de Maven ou d'un JDK sur la machine hôte.
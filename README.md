# Commandes Docker à exécuter :

## Communication via localhost (sans network)

### Création des images Backend et Frontend

```
docker build -t goals-backend-image .
```

```
docker build -t goals-front-image .
```

### Exécution des containers Backend et Frontend

```
// Ici pas besoin de mapper le port car la comm sera cross containers
docker run -d --rm -p 27017:27017 \
    --name goals-db  \
    mongodb
```

```
// Execution du container backend avec mapping du port pour le front
docker run --rm -p 80:80 \
    --name goals-backend \
    goals-backend-image
```

```
// Execution du container front avec mapping du port car accessible depuis un browser 
// donc pas besoin du network ici :
// Le front est exécuté sur le browser (requete entrante vers le container du backend)
docker run --rm -p 3000:3000 \
    --name goals-front \
    goals-front-image
```

## Utilisation du network, volumes et bind mounts pour la persistence des données et le live reload

## Création du network :**

```
docker network create goals-net
```

## Création des images

Voir plus haut

## Exécution des containers

```
// Container MongoDB avec user credentials et un bind mount pour persister les données
docker run -d --rm \
    --network goals-net \
    -e MONGO_INITDB_ROOT_USERNAME=user \
    -e MONGO_INITDB_ROOT_PASSWORD=password \
    -v goals-data:/data/db \
    --name goals-db 
    mongo
```

```
docker run --rm -p 80:80 \
    --name goals-backend 
    --network goals-net 
    -v logs-data:/app/logs // volume pour persitence temporaire 
    -v {absolute_path_to_project}:/app // bind mount pour maj du source du container 
    -v /app/node_modules // volume pour éviter override node_modules par host 
    -e MONGODB_USERNAME=user \
    -e MONGODB_PASSWORD=password \
    goals-backend
```

```
docker run --rm -p 3000:3000 \
    --name goals-front \
    --network goals-net \
    -v {absolute_path_to_project}/src:/app/src \
    -e WATCHPACK_POLLING=true  // permet l'auto reload des sources reactjs \
    -v /app/node_modules \
    goals-front
```

# Utilisation de Docker Compose :

```
// Démarrer tous les containers, avec forcing du build d'image et en détaché :
docker-compose up --build -d

// arréter les containers et les supprimers à leur arrêt :
docker-compose down

// Build d'image :
docker-compose build nom_service

// Execution de containers :
docker-compose run nom_service --rm -d -v ......

```

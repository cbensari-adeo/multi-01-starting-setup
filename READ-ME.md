Commandes Docker a exécuter :

**création du network :**
docker network create goals-net

**Ici pas besoin de mapper le port car la comm sera cross containers :**
docker run -d --rm --name goals-db --network goals-db mongodb

** Création de l'image backend :**
docker build -t goals-backend-image .

**Execution du container backend avec mapping du port pour le front**
Le front est exécuté sur le browser (requete entrante vers le container du backend) :
docker run --rm -p 80:80--name goals-backend --network goals-net goals-backend-image

**Création de l'image front :**
docker build -t goals-front-image .

**Execution du container front avec mapping du port**
Car accessible depuis un browser donc pas besoin du network ici :
docker run --rm -p 3000:3000 --name goals-front goals-front-image

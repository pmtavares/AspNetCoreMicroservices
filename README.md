# AspNetCoreMicroservices

### Getting mongodb docker
* The mongodb will be in docker container, in order to set up, go to https://hub.docker.com/ and search for mongo (https://hub.docker.com/_/mongo). 


### Execute the following command to download the image: 
```bash 
docker pull mongo
```

You will see in the cmd screen mongodb being downloaded

#### Run the container:

```bash 
docker run -d -p 27017:27017 --name aspnetrun-mongo mongo
```

#### Go into the container
```bash 
docker exec -it aspnetrun-mongo /bin/bash
```
* type: mongo
* Create new Database: use CatalogDb
* Create Collection: db.createCollection('Products')

#### Install

* MongoDB.Driver: 2.12.2


### Create docker image of api
* Before doing anything, go to VS menu, tools/options/Container tools/Docker compose, change the values to:
 - Pull required Docker images on project open: False
 - Pull updated Docker images on project open: Never
 - Remove containers on project close: True
 - Run containers on project open: False

 * Click with right button on the project, Add / Container Orchestrator Support
 * Target OS: Linux. A docker file is generated (docker-compose)
 * Go to the new created file docker-compose.yml and add the following line in order to get services from another container
 {
     catalogdb:
       image: mongo
 }
 
 * In the same file, add volume as below:
 volumes:
   mongo_data:

 * Go to new created docker-compose-override.yml file and add the following:

 {
services:
  catalogdb:
    container_name: catalogdb
    restart: always
    ports:
      - "27017:27017" 
    volumes:
      - mongo_data:/data/db

  catalog.api:
    container_name: catalog.api
    environment:
      - ASPNETCORE_ENVIRONMENT=Development
      - "CatalogDatabaseSettings:ConnectionString=mongodb://catalogdb:27017
    depends_on:
      - catalogdb
    ports:
      - "8000:80" //Our port is 8000
 }

 * After above, go to the folder where docker-compose is, open cmd and execute the bellow:
 > docker-compose -f docker-compose.yml -f docker-compose.override.yml up -d
 
 
 * Go to localhost:8080/swagger. You will see the application running

 Note: Be aware that we might face indentation issues
 
  * Remove dockers image:
 > docker-compose -f docker-compose.yml -f docker-compose.override.yml download

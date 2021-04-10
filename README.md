# AspNetCoreMicroservices

### Getting mongodb docker
* The mongodb will be in docker container, in order to set up, go to https://hub.docker.com/ and search for mongo (https://hub.docker.com/_/mongo). 
Execute the following command to download the image: docker pull mongo

You will see in the cmd screen mongodb being downloaded

#### Run the container:
>> docker run -d -p 27017:27017 --name aspnetrun-mongo mongo

#### Go into the container
```bash 
docker exec -it aspnetrun-mongo /bin/bash
```
* type: mongo
* Create new Database: use CatalogDb
* Create Collection: db.createCollection('Products')
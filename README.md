**Docker commands**

```
docker images
docker pull nginx:1.23
docker run -d nginx:1.23
docker logs 8cb613be00cf  # 8cb613be00cf meand container id
docker stop 8cb613be00cf  # 8cb613be00cf meand container id
docker run -d -p 90:80 nginx:1.23  # -p means port host_port:container_port
docker run --name web-server-ng -d -p 90:80 nginx:1.23
docker ps -a
docker start 7ce3de5c6b5d # 7ce3de5c6b5d means container id
docker rm 8a30bd0f3301 ## Delete container 8a30bd0f3301
docker rmi my-me-app:0.1 ## Delete image my-me-app:0.1
```

**Build docker images and push to dockerhub**

```
docker build -t node-app:0.1 .  ## . means Dockerfile location
docker run --name node-app-1 -d -p 90:3000 node-app:0.1
docker tag node-app:0.1 babu12f/node-app:0.1 ## Create new tag form image
Docker login # it will ask docker hub username and password
docker push babu12f/node-app:0.1
```
**exec command**
```
docker exec -it redis-1 /bin/sh # redis-1 is container name bin/sh or sh 
docker exec -it 5b3a5fecfe1e /bin/sh # 5b3a5fecfe1e is container id
```

**Docker networking**
```
docker network ls
docker network create mongo-network
```

**Start mongodb**
```
docker run -d \
> -p 27017:27017 \
> -e MONGO_INITDB_ROOT_USERNAME=admin \
> -e MONGO_INITDB_ROOT_PASSWORD=password \
> --name mongo-db \
> --net mongo-network \
> mongo
```

**Start mongo-express**
```
docker run -d \
> -p 8081:8081 \
> -e ME_CONFIG_MONGODB_ADMINUSERNAME=admin \
> -e ME_CONFIG_MONGODB_ADMINPASSWORD=password \
> -e ME_CONFIG_MONGODB_SERVER=mongo-db \
> --name mongo-express \
> --net mongo-network \
> mongo-express

====================
# access mongo-express from browser http://localhost:8081
# it will ask username and password
# username: admin
# password: pass
# you can see username and password in docker run command 
# "docker logs mongo-express" 
```

**Build and Start mongo and express app**
```
docker build -t my-me-app:0.1 .

docker run -d \
> -p 3000:3000 \
> --name me-app \
> --net docker_default \
> my-me-app:0.1
```

**Tag app image**
```
docker tag my-me-app:0.1 babu12f/me-app:0.1
docker push babu12f/me-app:0.1
```

## demo app - developing with Docker

All components are docker-based

### With Docker

#### To start the application

Step 1: Create docker network

    docker network create mongo-network 

Step 2: start mongodb

    docker run -d -p 27017:27017 -e MONGO_INITDB_ROOT_USERNAME=admin -e MONGO_INITDB_ROOT_PASSWORD=password --name mongodb --net mongo-network mongo    

Step 3: start mongo-express

    docker run -d -p 8081:8081 -e ME_CONFIG_MONGODB_ADMINUSERNAME=admin -e ME_CONFIG_MONGODB_ADMINPASSWORD=password --net mongo-network --name mongo-express -e ME_CONFIG_MONGODB_SERVER=mongodb mongo-express   

_NOTE: creating docker-network in optional. You can start both containers in a default network. In this case, just emit `--net` flag in `docker run` command_

Step 4: open mongo-express from browser

    http://localhost:8081

Step 5: create `user-account` _db_ and `users` _collection_ in mongo-express

Step 6: Start your nodejs application locally - go to `app` directory of project

    npm install 
    node server.js

Step 7: Access you nodejs application UI from browser

    http://localhost:3000

### With Docker Compose

#### To start the application

Step 1: start mongodb and mongo-express

    docker-compose -f docker-compose.yaml up

_You can access the mongo-express under localhost:8080 from your browser_

Step 2: in mongo-express UI - create a new database "my-db"

Step 3: in mongo-express UI - create a new collection "users" in the database "my-db"

Step 4: start node server

    npm install
    node server.js

Step 5: access the nodejs application from browser

    http://localhost:3000

#### To build a docker image from the application

    docker build -t my-app:1.0 .       

The dot "." at the end of the command denotes location of the Dockerfile.

**Dockcer commands**

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
```

**Build docker images and push to dockerhub**

```
docker build -t node-app:0.1 .  ## . means Dockerfile location
docker run --name node-app-1 -d -p 90:3000 node-app:0.1
docker tag node-app:0.1 babu12f/node-app:0.1 ## Create new tag form image
Docker login # it will ask docker hub username and password
docker push babu12f/node-app:0.1
```

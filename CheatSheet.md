# CheatSheet

Install docker
`wget -qO- https://get.docker.com/ | sh`

Start Fedora container  
`docker run -it fedora bash`

Start Wildfly with port-forwarding and name  
`docker run -d -p 8081:8080 -p 9991:9990 --name roomservice jboss/wildfly`

Stop and remove container  
`docker stop roomservice && docker rm roomservice`

Stop and remove all containers  
`docker stop $(docker ps -a -q) && docker rm $(docker ps -a -q)`

Remove all images  
`docker rmi -f images $(docker images -a -q)`

Build image  
`docker build --tag=wildfly /var/lib/docker/dockerfiles/wildfly`

Start datastore container  
`docker run -di \`  
`-v /var/lib/docker/static-volume/roomservice-db:/var/lib/postgresql/data \`  
`--name roomservice-db-datastore postgres-datastore`

Connect with datastore container  
`docker run -d \`  
`-e POSTGRES_PASSWORD=postgres \`  
`-p 5432:5432 \`  
`--volumes-from roomservice-db-datastore \`  
`--name roomservice-db postgres-db`

Link containers  
`docker run -p 8081:8080 -p 9991:9990 -d \`  
`--name roomservice --link roomservice-db:roomservice-db wildfly`

Open bash in container  
`docker exec -it roomservice bash`

Copy files from container  
`docker cp roomservice:/opt/jboss/wildfly/standalone/configuration /var/lib/docker/static-volume/roomservice-configuration`

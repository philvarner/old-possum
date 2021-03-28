# Docker

## Resources

* tbd

## Stop All

docker stop $(docker ps -aq)


## Shell in a container

docker exec -it container_name /bin/bash

(or /bin/ash if alpine)

## Execute command in container

docker exec -it my_container sh -c "echo a && echo b"

## Logs

docker logs -f aa1fb0d033b9


http://phase2.github.io/devtools/common-tasks/ssh-into-a-container/
* Use the command docker exec -it <container name> /bin/bash to get a bash shell in the container

How do I SSH into a running container
http://phase2.github.io/devtools/common-tasks/ssh-into-a-container/


## TODO


Docker

docker run -it --entrypoint /bin/bash <image>

run wget -> replace with ADD URI filename

alpine apk
https://wiki.alpinelinux.org/wiki/Alpine_Linux_package_management#Add_a_Package

Try ECS for k8s?

docker run -d -p 8080:8080 -e WHO="Sean and Karl" \
example/docker-node-hello:latest


docker exec -it <container name> /bin/bash
docker run -it <container name> <command>

mount file system -v /mnt/session_data:/data 
--read-only=true 

-i interactive
-t pseudo-tty

--rm delete container on exit

--tmpfs tmp filesystem

--read-only=true --tmpfs   /tmp:rw,noexec,nodev,nosuid,size=256M 

docker ps -a

pause/unpause

docker stop
docker kill


docker rmi - remove image

docker system prune


delete all containers
docker rm $(docker ps -a -q)

delete all images
docker rmi $(docker images -q -)

docker system prune -a
	

-d daemon mode

docker inspect <id>

docker exec

docker top

https://derickbailey.com/2017/03/09/selecting-a-node-js-image-for-docker/

- principle - base all on alpine if possible

define ENTRYPOINT and CMD rather than just CMD

https://ropenscilabs.github.io/r-docker-tutorial/04-Dockerhub.html
docker tag 029df420bc7b s22s/geo-swak:v2
docker push s22s/geo-swak

docker rmi -f $(docker images | grep "<none>" | awk "{print \$3}") && docker ps -a -q | xargs docker rm -f`
`docker system prune` 

docker system prune --filter "until=200h"

journalctl -u docker.service
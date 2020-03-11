# Docker

docker doesn't start

```
rm -rf /var/lib/docker/*
```

list images

```
docker images
```
list images in registry  
```
curl -k https://myregistry:5000/v2/_catalog | jq
```

list tags  
```
curl -k https://myregistry:5000/v2/base/alpine/tags/list | jq
```
```
export DOCKER_HOST=tcp://x10486.rz2012.adm.denic.de:2375
```

```
docker build - < Jenkins_Docker
```

build image from Dockerfile

```
docker build --tag=test -f ./Dockerfile .apiVersion: v1
kind: PersistentVolume
metadata:
  name: nfs-pgdata-prod
  labels:
    app: postgresql
    env: default
spec:
  capacity:
    storage: 2G
  accessModes:
    - ReadWriteOnce
  nfs:
    server: kube-master-01.internal.ng.freedom-id.de
    path: "/mnt/nfs-ng-fra1-part2/postgres/production/pg-master"
```

list all containers
```
docker ps -a
```

run new container
```
docker run <image>
```

start existing container
```
docker start <containerid>
```

map host network (even localhost) to container
```
docker run --net="host" <image>
```


```
docker exec -it <container> /bin/sh

```
docker run --entrypoint "/usr/bin/top" -it <image>
```
docker delete image
```

```
docker rm $(docker ps -a -q)
docker rmi <image>
```

## move container, backup

```bash
docker commit <container>
docker save <container> | gzip > myimage.tar.gz
gunzip -c myimage.tar.gz | docker load
```

## show run command from container

```bash
docker run --rm -v /var/run/docker.sock:/var/run/docker.sock assaflavie/runlike <container>
```



### expose port

```
--expose=22
```

```
docker exec -ti <container_name> bash
```


#### docker own registry
```
# copy your cert to /etc/docker/certs.d/<docker registry name>
mkdir -p /etc/docker/certs.d/docker-registry.cloud.denic.de
cp docker-registry.crt /etc/docker/certs.d/docker-registry.cloud.denic.de/
```

### push image

```
docker commit <container> <image>
docker tag <image> <registry image>
docker push <registry image>

docker commit eb46aefe135c ipnanny/ipnanny:1.0
docker tag ipnanny/ipnanny:1.0 dcr.adm.denic.de/ipnanny/ipnanny:1.0
docker push dcr.adm.denic.de/ipnanny/ipnanny:1.0
```

### show mounted path
```
docker inspect -f "{{ .Mounts }}" <container>
```


#### delete registry
```
curl -i -k -X GET https://kube-registry.freedom-id.de/v2/tools/thorstek-postfix/manifests/latest

```
https://forums.docker.com/t/delete-repository-from-v2-private-registry/16767


##### docker Xorg
```
xhost +
docker run -ti --memory 2gb --rm -e DISPLAY=unix:0 -v /tmp/.X11-unix:/tmp/.X11-unix  -v=/dev/dri:/dev/dri:rw myimage mycmd
```

#### tools
- ctop monitors the containers on the hosting server
- photon linux distro for hosting containers. It comes with security flags enabled and docker already included

#### docker registry cleanup
https://github.com/fraunhoferfokus/deckschrubber


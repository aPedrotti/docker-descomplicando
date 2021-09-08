#       ===== DOCKER SWARM =====
## Create Environment
**Requirements:**
- docker
- vagrant

Run a 'vagrant up' to setup a cluster with 1 manager / 2 workers (check Vagranfile for details)

## Release notes
#### What it is:
It has a MANAGER a WORKER to balance workloads evenly (as per config)

OBS: Manager can execute containers, but it is not recommended.


You can promote wokers to managers (co-leader) by:
`$ docker node promote worker_name`

To undone:
`$ docker node demote worker_name`

To remove a worker from cluster:
`$ docker swarm leave`

If it a co-leader has to be forced '-f'. And tnen:
`$ docker swarm rm node_name`

### - Conecting servers - Tokens:
To get joins tokens:
`$ docker swarm join-token worker/manager`

To update/change token:
`$ docker swarm join-token --rotate worker/manager`


## - Updating Nodes

You can update condition of node in the cluster (active, pause, drain)

You can add or remove labels

You can update node role (worker or manager)

`$ docker node update --help`

`$ docker node update --avaibility pause worker`

## - Creating Services / Containers
`$ docker service create --name webserver --replicas 3 -p 8080:80 nginx `

Checking list of created services: 
`$ docker service ls`

Checking where your service was created: 
`$ docker service ps webserver `

Scaling your service:
`$ docker service scale webserver --replicas=10`

## - Volumes 

Create your local volume:
`$ docker volume create myvol `

Add custom index: 
`$ echo "MY CUSTOM INDEX" > /var/lib/docker/volumes/myvol/_data/index.html`

Mount your volume into your service:
`$ docker service create --name web --replicas 3 -p 8080:80 --mount type=volume,src=myvol,dst=/usr/share/nginx/html nginx`

*Your volume will be shared with nodes, but not the content*

*To solve that you can use a nfs-server*

```
apt install nfs-server -y
echo "/var/lib/docker/volumes/myvol *(rw,sync,subtree_check)" >> /etc/exports
exportfs -ar
```

Install nfs-common in the workers and then mount in the same dir:
```
apt-get install -y nfs-common
mount -t nfs 10.10.10.10:/var/lib/docker/volumes/myvol /var/lib/docker/volumes/myvol
```

### - Networking

`$ docker network create -d overlay my-swarm-net`


`$ docker service create --name nginx --network my-swarm-net -p 8080:80 nginx`

`$ docker service create --name nginx2 --network my-swarm-net -p 8088:80 nginx`

Inspecting services you will see same refernce by VirtualIPs[1].NetworkID

From any container you can *curl* the service name

You can add different networks to services: `$ docker service update --network-add my-new-net nginx2`

And add/rm published ports: `$ docker service update --publish-add 9999 nginx2`


<!--
### - Rolling(in) RollingOut
-->



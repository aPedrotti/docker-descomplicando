#       ===== DOCKER COMPOSE =====

Starting multiples containers / services

docker-compose:

up - create and start env

ps - list 'services' (this service is different from docker swarm)


## STACK

Um ou mais services

`docker stack deploy giropops --compose-file docker-compose.yml`
`docker stack services giropops`
`docker stack rm giropops`


### The Difference between:
```
$ docker-compose -f docker-compose up
$ docker stack deploy -c docker-compose.yml somestackname 
```

> Docker stack is ignoring “build” instructions. You can’t build new images using the stack commands. It need pre-built images to exist. So docker-compose is better suited for development scenarios.
> 
> There are also parts of the compose-file specification which are ignored by docker-compose or the stack commands. (Search for “ignore” on that page to look through more details).
> 
> Docker Compose is a Python project. Originally, there was a Python project known as fig which was used to parse fig.yml files to bring up - you guessed it - stacks of Dock`r containers. Everybody loved it, especially the Docker folks, so it got reincarnated as docker compose to be closer to the product. But it was still in Python, operating on top of the Docker Engine.
> 
> Internally, it uses the Docker API to bring up containers according to a specification. You still have to install docker-compose separately to use it with Docker on your machine.
> 
> The Docker Stack functionality, is included with the Docker engine. You don’t need to install additional packages to use it Deploying docker stacks is part of the swarm mode. It supports the same kinds of compose files, but the handling happens in Go code, inside of the Docker Engine. You also have to create a one-machine “swarm” before being able to use the stack command, but that’ not a big issue.
> 
> Docker stack does not support docker-compose.yml files which are written according to the version 2 specification. It has to be the most recent one, which is 3 at the time of writing, while Docker Compose still can handle versions 2 and 3 without problems.
#!/bin/bash

## What it is
Create a server with docker to manage remotely 

### === Create locally
`$ docker-machine create --driver virtualbox default`

To point your binary to your server

`$ eval $(docker-machine env <vmname>)`

### == AWS
(configure your credentials before)

`$ docker-machine create --driver amazonec2 aws01 --amazonec2-vpc-id vpc-0a94e5e901a30dda8`

OBS: Required passing VPC if there isn't a default named one

checkout more comands from official Doc

<https://docs.docker.com/machine/drivers/aws/#options>

NOT WORKING = OBS: You can pass your own SSH-Key (avoids problems waiting from aws's key)
```
$ docker-machine create \
--driver amazonec2 \
--amazonec2-vpc-id vpc-0a94e5e901a30dda8 \
--amazonec2-ssh-keypath ~/.ssh/my_local \
aws01
```



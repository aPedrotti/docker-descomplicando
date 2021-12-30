# Sharing volumes

```
docker create --name dbdados -v /data centos
docker run -d \
--name pgsql1 \
-p 5432:5432  \
--volumes-from dbdados \
-e POSTGRESQL_USER=docker \
-e POSTGRESQL_PASS=docker \
-e POSTGRESQL_DB=docker \
kamui/postgresql
```
```
docker run -d \
--name pgsql2 \
-p 5433:5432  \
--volumes-from dbdados \
-e POSTGRESQL_USER=docker \
-e POSTGRESQL_PASS=docker \
-e POSTGRESQL_DB=docker \
kamui/postgresql
```
# create a volumes and create these pgsql from this volume
```
docker volume create mydatadir
docker run -d \
--name pgsql1 \
-p 5432:5432  \
--mount type=volume,src=mydatadir,dst=/data \
-e POSTGRESQL_USER=docker \
-e POSTGRESQL_PASS=docker \
-e POSTGRESQL_DB=docker \
kamui/postgresql```
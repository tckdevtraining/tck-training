# Setup Redis back-end
Here are some step to have a Redis database back-end locally.

## Start a Redis docker container
- Pull Redis 6.2.5 docker image from dockerhub
  - https://hub.docker.com/_/redis

```
bash~$ docker pull redis:6.2.5
6.2.5: Pulling from library/redis
Digest: sha256:cd0c68c5479f2db4b9e2c5fbfdb7a8acb77625322dd5b474578515422d3ddb59
Status: Downloaded newer image for redis:6.2.5
docker.io/library/redis:6.2.5
```

- Start container with persistent storage

```
bash~$ mkdir ~/redis_data
bash~$ docker run --name redis-tck-training -v ~/redis_data:/data -d redis redis-server --appendonly yes -p 6379:6379
d0e6bff28e67b3d27bdc89041768f327d51cfa3590976b0f6d8c9d71f9caf8a2
bash~$ ls ~/redis_data/
appendonly.aof
bash~$ docker ps | grep redis
d0e6bff28e67   redis   "docker-entrypoint.s…"   15 seconds ago   Up 14 seconds   6379/tcp   redis-tck-training
```

## Connect to Redis container with a gui client
I will use _Redis Desktop Manager_ (_RDM_) as graphical client to connect to the redis back-end:

```
bash~$ sudo snap install redis-desktop-manager
```
Once it is installed you can launch it. There is currently no password defined. you just have to connect with '_localhost_' and port '_6379_':
![](../.assets/rdm_connection.png)

and play with the default 16 databases (_db0_, _db1_, ..., _db15_):
- https://redis.io/topics/data-types-intro

I clicked on _db0_ then '_Open console_' and execute few commands. To see them clien on '_Reload keys in database_':
![](../.assets/rdm_console.png)

Great ! We have a functional Redis database !

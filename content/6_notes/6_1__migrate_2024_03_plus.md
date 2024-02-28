---
title: 'Migration, 2024-03+'
---

After the end of February 2023, the project uses zookeeper for service discovery and migrates to a new docker
build system.  This is a fairly large change, and requires a few manual migration steps to keep using an existing 
installation.

## Easy way: 

Do a clean install somewhere and copy the `docker-compose.yml` and `env/service.env` to your existing install.

## Hard way: 

1.  Add a zookeeper service to the docker-compose file.

```yml
services:
...
  zookeeper:
    image: zookeeper
    container_name: "zookeeper"
    restart: always
    ports:
      - "127.0.0.1:2181:2181"
    networks:
      - wmsa
```

2. In the docker-compose file, add an item `zookeeper` to the `depends_on` for `x-svc`, `x-p1` (and `x-p2` if exists). 
(this is strictly speaking not necessary, but if you don't do this you'll encounter a period of transient connection 
issues every time you restart the system.)

3. In the docker-compose file, remove any health checks from `x-svc`, `x-p1` (and `x-p2` if exists).

4. In the docker-compose file, find and remove any blocks that look like this:

```yml
    depends_on:
        control-service:
          condition: service_healthy
```

5. In `env/service.env`: Add a line `ZOOKEEPER_HOSTS="zookeeper:2181"`

6. In `env/service.env`: Rename `JAVA_OPTS` to `JDK_JAVA_OPTIONS`.


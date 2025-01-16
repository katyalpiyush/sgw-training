# SGW Training

## Setup

### 1. Single Node Setup

> [!NOTE]
> This setup will not use any custom docker networks and it will contain a basic 1-1 mapping of CB and SGW

- Create CB container:

```
docker run -d --name cb_0 -p 8091:8091 couchbase:latest
```

- Initialize the cluster and create a bucket

- Inspect and get the IP address of the node

```
docker inspect $(docker ps -aq) | jq -c '.[] | [.Name, .NetworkSettings.Networks[].IPAddress]'
```

- Ensure that the IP address in `config1.json` is same as the IP address obtained from the above command

- Start SGW container:

```
docker run -d --name sgw -p 4984-4986:4984-4986 -v ./config1.json:/etc/sync_gateway/config.json couchbase/sync-gateway:enterprise
```

- Test if SGW is running by running the following curl command:

```
curl localhost:4984
```

### 2. Multi Node Setup

> [!Note]
> This setup will use any docker networks

- Create docker network

```

docker network create -d bridge --subnet 10.0.0.0/8 my-bridge-network

```

- Start docker containers for Couchbase:

```

docker compose up cb-server1 cb-server2 cb-server3 -d

```

- Initialize the cluster and create a bucket

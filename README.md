# Self-hosted Docker Registry (standalone binary)

The purpose of self-hosted Docker Registry is to organize the same workflow as with the [DockerHub](https://hub.docker.com/), but privately, e.g. for a company developing Docker containers for internal use.

This is a docker registry application v2.8.2, obtained from the official Docker container by doing `docker pull registry`.

Since the application is linked statically, we can use it natively on any x86_64 Linux system.


## Deployment

From this repository folder execute:

```
./bin/registry serve ./etc/docker/registry/config.yml
```


## Usage

Users should connect to the node, which run this registry over an SSH tunnel. This tunnel performs the regular user's authentication with the same login/password or SSH key, which is used for normal login. The tunnel should be kept open until the docker push/pull operations are finished:

```
ssh -L 5000:localhost:5000 myserver -N
```


## Testing

In order to push to a local registry, we must create a tag for the target container starting with `localhost:5000/`:

```
docker tag registry:latest localhost:5000/registry:latest
docker push localhost:5000/registry:latest
The push refers to repository [localhost:5000/registry]
85f82aceeda3: Pushed
f79c4d8837b6: Pushed
90d6ca1e837f: Pushed
f4285c491509: Pushed
4693057ce236: Pushed
latest: digest: sha256:da1fbcd13a7ddc77d0d964a5c5c4cb707b5d440a028b0b42fe574b9e99077e27 size: 1363
```

Pulling container from a local registry is done the same way:

```
docker pull localhost:5000/registry:latest
latest: Pulling from registry
Digest: sha256:da1fbcd13a7ddc77d0d964a5c5c4cb707b5d440a028b0b42fe574b9e99077e27
Status: Image is up to date for localhost:5000/registry:latest
localhost:5000/registry:latest
```


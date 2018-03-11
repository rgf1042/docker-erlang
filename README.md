# docker-erlang
Docker images for running erlang software

## Requirements
We must have installed those packages:
- **docker engine**: version 1.13 or above

And that should be enough to run our image

## How to use them
### Prepare network
It's useful to test erlang software simulating a "real" network, so first we can create a bridged network and attach it in all erlang containers:
```
docker network create -d bridge sdx-net
```

### Running containers without X11 support

We assume that the user has created one bridged network and he has some development sources to include in the container.

```
docker run -it -v "$PWD":/home/development/src --network sdx-net rgf1042/erlang:base
```
Now we have one container running inside sdx-net bridged network and with all current directory data attached in /home/development/src inside the container.
With ```--name``` flag it's possible to include a personalized name (it could be interesting if you want to identify the container later).

### Running containers with X11 support
Sometimes it's necessary to add graphics support in erlang applications. Fortunately, docker supports mounting devices as volumes. In this case we attach X11 server related files to the container and then it sends windows directly to the host server.
```
docker run -it -e DISPLAY=$DISPLAY -v "$PWD":/home/development/src \
  -v /tmp/.X11-unix:/tmp/.X11-unix --network sdx-net rgf1042/erlang:base
```

### UID and GID stuff
If your host user id (UID) and group id (GID) are different to 1000 (default values) yo have to set environment variables ```$HOST_USER_ID``` and ```$HOST_USER_GID``` with the correct values.

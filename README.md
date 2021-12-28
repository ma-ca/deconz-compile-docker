# Compile deconz-rest-plugin with docker

Compile the deconz-rest-plugin https://github.com/dresden-elektronik/deconz-rest-plugin using a docker container with preinstalled `deconz-dev` package and all dependencies. The docker images are available for different arm32v6/arm32v7/arm64v8 platforms and are all running on Linux x86_64. Copy the compiled binary `libde_rest_plugin.so` 

The multiarch/qemu-user-static docker images https://github.com/multiarch/qemu-user-static enables running docker container for arm32v6/arm32v7/arm64v8 architectures. The docker containers run on x86_64 Linux.

The following docker images are available:

- ghcr.io/ma-ca/deconz-dev-buster-arm32v6
- ghcr.io/ma-ca/deconz-dev-buster-arm32v7
- ghcr.io/ma-ca/deconz-dev-buster-arm64v8
- ghcr.io/ma-ca/deconz-dev-bullseye-arm32v6
- ghcr.io/ma-ca/deconz-dev-bullseye-arm32v7
- ghcr.io/ma-ca/deconz-dev-bullseye-arm64v8

Compile deconz-rest-plugin for Raspberrypi arm32v7

```
git clone https://github.com/dresden-elektronik/deconz-rest-plugin.git

docker run --rm --privileged multiarch/qemu-user-static --reset -p yes
docker pull ghcr.io/ma-ca/deconz-dev-bullseye-arm32v7

docker run --rm -v $(pwd):/deconz -w /deconz ghcr.io/ma-ca/deconz-dev-bullseye-arm32v7 \
sh -c "cd deconz-rest-plugin && qmake && make"
```

Create docker images locally

```
docker build -t deconz-dev-buster-arm32v6 -f Dockerfile-deconz-dev-buster-arm32v6 .
docker build -t deconz-dev-buster-arm32v7 -f Dockerfile-deconz-dev-buster-arm32v7 .
docker build -t deconz-dev-buster-arm64v8 -f Dockerfile-deconz-dev-buster-arm64v8 .
docker build -t deconz-dev-bullseye-arm32v6 -f Dockerfile-deconz-dev-bullseye-arm32v6 .
docker build -t deconz-dev-bullseye-arm32v7 -f Dockerfile-deconz-dev-bullseye-arm32v7 .
docker build -t deconz-dev-bullseye-arm64v8 -f Dockerfile-deconz-dev-bullseye-arm64v8 .
```

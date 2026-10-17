# Compile deconz-rest-plugin with podman

Compile the deconz-rest-plugin https://github.com/dresden-elektronik/deconz-rest-plugin using container with preinstalled `*-dev` packages and all dependencies. The images are available for different arm32v6/arm32v7/arm64v8 platforms and are all running on Linux x86_64 and arm64 platforms. Copy the compiled binary `libde_rest_plugin.so` to `/usr/share/deCONZ/plugins/` directory.

The `tonistiigi/binfmt` image enables running container for arm32v6/arm32v7/arm64v8 architectures. The containers run on x86_64/arm64 Linux and macOS silicon.

The following images are available:

Qt5 images:
- ghcr.io/ma-ca/deconz-dev-trixie-arm32v6
- ghcr.io/ma-ca/deconz-dev-trixie-arm32v7
- ghcr.io/ma-ca/deconz-dev-trixie-arm64v8
- ghcr.io/ma-ca/deconz-dev-trixie

Qt6 images:
- ghcr.io/ma-ca/deconz-dev-qt6-trixie-arm32v6
- ghcr.io/ma-ca/deconz-dev-qt6-trixie-arm32v7
- ghcr.io/ma-ca/deconz-dev-qt6-trixie-arm64v8
- ghcr.io/ma-ca/deconz-dev-qt6-trixie

Compile deconz-rest-plugin for Raspberrypi arm32v7

```
git clone https://github.com/dresden-elektronik/deconz-rest-plugin.git

cd deconz-rest-plugin

sudo podman run --rm --privileged tonistiigi/binfmt --install all

podman pull ghcr.io/ma-ca/deconz-dev-bookworm-arm32v7

podman run --rm -v $(pwd):/deconz-rest-plugin -w /deconz-rest-plugin \
ghcr.io/ma-ca/deconz-dev-bookworm-arm32v7
```

Create images locally, running rootless Podman

Platform                | Raspberry Pi      |
--------                | -------------     |
arm32v6 (linux/arm/v6)  | 1 / Zero / Zero W |
arm32v7 (linux/arm/v7)  | 2 / 3 / Zero 2 W / 4 / 5 |
arm64 (linux/arm64)     | 2 / Zero 2 W / 3 / 4 / 5 |

Dockerfile                          | Debian Release                           | QT Version | Platform Target = Host | Target |
-------------                       | --------------                           | ---------- | ---------------------- | ------ |
Dockerfile-deconz-dev-qt5-legacy    | Buster, Bullseye                         | 5          | yes                    | -
Dockerfile-deconz-dev-qt5           | Bookworm, Trixie                         | 5          | yes                    | -
Dockerfile-deconz-dev-qt5-arm1176   | Buster, Bullseye, Bookworm, Trixie       | 5          | no                     | arm32v6
Dockerfile-deconz-dev-qt5-arm       | Buster, Bullseye, Bookworm, Trixie       | 5          | no                     | arm32v7, arm64v8
Dockerfile-deconz-dev-qt6           | Bookworm, Trixie                         | 6          | yes                    | -
Dockerfile-deconz-dev-qt6-arm1176   | Bookworm, Trixie                         | 6          | no                     | arm32v6
Dockerfile-deconz-dev-qt6-arm       | Bookworm, Trixie                         | 6          | no                     | arm32v7, arm64v8

Qt5 — arm32v6 (arm1176)

```
podman build --cap-add=SYS_ADMIN \
--build-arg RELEASE=buster \
--build-arg TARGET_ARCH=armhf \
--build-arg TARGET_CPU="arm1176" \
--build-arg REPOSITORY="http://archive.debian.org/debian-archive/debian" \
-f Dockerfile-deconz-dev-qt5-arm1176 -t deconz-dev-buster-arm32v6 .
```

```
podman build --cap-add=SYS_ADMIN \
--build-arg RELEASE=bullseye \
--build-arg TARGET_ARCH=armhf \
--build-arg TARGET_CPU="arm1176" \
--build-arg REPOSITORY="http://raspbian.raspberrypi.com/raspbian/" \
-f Dockerfile-deconz-dev-qt5-arm1176 -t deconz-dev-bullseye-arm32v6 .
```

```
podman build --cap-add=SYS_ADMIN \
--build-arg RELEASE=bookworm \
--build-arg TARGET_ARCH=armhf \
--build-arg TARGET_CPU="arm1176" \
--build-arg REPOSITORY="http://raspbian.raspberrypi.com/raspbian/" \
-f Dockerfile-deconz-dev-qt5-arm1176 -t deconz-dev-bookworm-arm32v6 .
```

```
podman build --cap-add=SYS_ADMIN \
--build-arg RELEASE=trixie \
--build-arg TARGET_ARCH=armhf \
--build-arg TARGET_CPU="arm1176" \
--build-arg REPOSITORY="http://raspbian.raspberrypi.com/raspbian/" \
-f Dockerfile-deconz-dev-qt5-arm1176 -t deconz-dev-trixie-arm32v6 .
```

Qt5 — arm32v7

```
podman build --cap-add=SYS_ADMIN \
--build-arg RELEASE=buster \
--build-arg TARGET_ARCH=armhf \
--build-arg REPOSITORY="http://archive.debian.org/debian-archive/debian" \
-f Dockerfile-deconz-dev-qt5-arm -t deconz-dev-buster-arm32v7 .
```

```
podman build --cap-add=SYS_ADMIN \
--build-arg RELEASE=bullseye \
--build-arg TARGET_ARCH=armhf \
--build-arg REPOSITORY="http://raspbian.raspberrypi.com/raspbian/" \
-f Dockerfile-deconz-dev-qt5-arm -t deconz-dev-bullseye-arm32v7 .
```

```
podman build --cap-add=SYS_ADMIN \
--build-arg RELEASE=bookworm \
--build-arg TARGET_ARCH=armhf \
--build-arg REPOSITORY="http://raspbian.raspberrypi.com/raspbian/" \
-f Dockerfile-deconz-dev-qt5-arm -t deconz-dev-bookworm-arm32v7 .
```

```
podman build --cap-add=SYS_ADMIN \
--build-arg RELEASE=trixie \
--build-arg TARGET_ARCH=armhf \
--build-arg REPOSITORY="http://raspbian.raspberrypi.com/raspbian/" \
-f Dockerfile-deconz-dev-qt5-arm -t deconz-dev-trixie-arm32v7 .
```

Qt5 — arm64v8

```
podman build --cap-add=SYS_ADMIN \
--build-arg RELEASE=buster \
--build-arg TARGET_ARCH=arm64 \
--build-arg REPOSITORY="http://archive.debian.org/debian-archive/debian" \
-f Dockerfile-deconz-dev-qt5-arm -t deconz-dev-buster-arm64v8 .
```

```
podman build --cap-add=SYS_ADMIN \
--build-arg RELEASE=bullseye \
--build-arg TARGET_ARCH=arm64 \
--build-arg REPOSITORY="http://deb.debian.org/debian" \
-f Dockerfile-deconz-dev-qt5-arm -t deconz-dev-bullseye-arm64v8 .
```

```
podman build --cap-add=SYS_ADMIN \
--build-arg RELEASE=bookworm \
--build-arg TARGET_ARCH=arm64 \
--build-arg REPOSITORY="http://deb.debian.org/debian" \
-f Dockerfile-deconz-dev-qt5-arm -t deconz-dev-bookworm-arm64v8 .
```

```
podman build --cap-add=SYS_ADMIN \
--build-arg RELEASE=trixie \
--build-arg TARGET_ARCH=arm64 \
--build-arg REPOSITORY="http://deb.debian.org/debian" \
-f Dockerfile-deconz-dev-qt5-arm -t deconz-dev-trixie-arm64v8 .
```

Qt5 — native x86_64 / arm64

```
podman build \
--build-arg RELEASE=bookworm \
-f Dockerfile-deconz-dev-qt5 -t deconz-dev-bookworm .
```

```
podman build \
--build-arg RELEASE=trixie \
-f Dockerfile-deconz-dev-qt5 -t deconz-dev-trixie .
```

Qt6 — arm32v6 (arm1176)

```
podman build --cap-add=SYS_ADMIN \
--build-arg RELEASE=bookworm \
--build-arg TARGET_ARCH=armhf \
--build-arg TARGET_CPU="arm1176" \
--build-arg REPOSITORY="http://raspbian.raspberrypi.com/raspbian/" \
-f Dockerfile-deconz-dev-qt6-arm1176 -t deconz-dev-qt6-bookworm-arm32v6 .
```

```
podman build --cap-add=SYS_ADMIN \
--build-arg RELEASE=trixie \
--build-arg TARGET_ARCH=armhf \
--build-arg TARGET_CPU="arm1176" \
--build-arg REPOSITORY="http://raspbian.raspberrypi.com/raspbian/" \
-f Dockerfile-deconz-dev-qt6-arm1176 -t deconz-dev-qt6-trixie-arm32v6 .
```

Qt6 — arm32v7

```
podman build --cap-add=SYS_ADMIN \
--build-arg RELEASE=bookworm \
--build-arg TARGET_ARCH=armhf \
--build-arg REPOSITORY="http://raspbian.raspberrypi.com/raspbian/" \
-f Dockerfile-deconz-dev-qt6-arm -t deconz-dev-qt6-bookworm-arm32v7 .
```

```
podman build --cap-add=SYS_ADMIN \
--build-arg RELEASE=trixie \
--build-arg TARGET_ARCH=armhf \
--build-arg REPOSITORY="http://raspbian.raspberrypi.com/raspbian/" \
-f Dockerfile-deconz-dev-qt6-arm -t deconz-dev-qt6-trixie-arm32v7 .
```

Qt6 — arm64v8

```
podman build --cap-add=SYS_ADMIN \
--build-arg RELEASE=bookworm \
--build-arg TARGET_ARCH=arm64 \
--build-arg REPOSITORY="http://deb.debian.org/debian" \
-f Dockerfile-deconz-dev-qt6-arm -t deconz-dev-qt6-bookworm-arm64v8 .
```

```
podman build --cap-add=SYS_ADMIN \
--build-arg RELEASE=trixie \
--build-arg TARGET_ARCH=arm64 \
--build-arg REPOSITORY="http://deb.debian.org/debian" \
-f Dockerfile-deconz-dev-qt6-arm -t deconz-dev-qt6-trixie-arm64v8 .
```

Qt6 — native x86_64 / arm64

```
podman build \
--build-arg RELEASE=bookworm \
-f Dockerfile-deconz-dev-qt6 -t deconz-dev-qt6-bookworm .
```

```
podman build \
--build-arg RELEASE=trixie \
-f Dockerfile-deconz-dev-qt6 -t deconz-dev-qt6-trixie .
```

On macOS

Prerequisites & Background

Apple Silicon (M1/M2/M3/M4) CPUs do not support 32-bit ARM execution mode (AArch32) at the hardware/hypervisor level.

Fedora CoreOS (the Linux VM inside Podman Machine) includes `/usr/bin/qemu-arm-static` out of the box, but omits the ARM32 binfmt_misc registration rule on AArch64.

Rootless user namespaces in Podman block cross-architecture binary translation unless Podman runs using the root connection socket.

```bash
# 1. Register qemu-arm-static persistently in the VM
podman machine ssh "sudo sh -c 'echo \":qemu-arm:M::\x7fELF\x01\x01\x01\x00\x00\x00\x00\x00\x00\x00\x00\x00\x02\x00\x28\x00:\xff\xff\xff\xff\xff\xff\xff\x00\xff\xff\xff\xff\xff\xff\xff\xff\xfe\xff\xff\xff:/usr/bin/qemu-arm-static:F\" > /etc/binfmt.d/qemu-arm-static.conf && systemctl restart systemd-binfmt'"

# 2. Switch Podman default connection to root
podman system connection default podman-machine-default-root

# 3. Test ARMv6 container execution
podman run --rm -it --platform linux/arm/v6 arm32v6/alpine uname -m
```

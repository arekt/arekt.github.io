### LinuxKit - Containers all the way down

Arek Turlewicz
(アレック）

@a_turl

---

## Introduction

  ```


  # whoami
  ```

---

  ```

  # whoami
  arekt
  ```

---

  - Rails/Ruby developer
  - AWS Cloud Services
  - I love eInk devices and RaspberryPi

---

## What is LinuxKit?

  - Open Containers Initiative
  - https://www.opencontainers.org/

  - RunC (runtime-spec)
  - ContainerD (image-spec)

---

### Do you have docker installed on your Mac?

  If so, try to type in your terminal

```
screen ~/Library/Containers \
  /com.docker.docker/Data/vms/0/tty
```

---

<img src="linuxkit/images/linuxkit-terminal.png">

---

```
docker-desktop:/tmp# ctr -n services.linuxkit tasks ls
TASK                     PID     STATUS
host-timesync-daemon     1177    RUNNING
sntpc                    1278    RUNNING
socks                    1365    STOPPED
write-and-rotate-logs    1558    RUNNING
acpid                    1029    RUNNING
diagnose                 1072    RUNNING
docker                   1122    RUNNING  <-------- docker is here
kmsg                     1224    RUNNING
trim-after-delete        1425    RUNNING
vpnkit-forwarder         1475    RUNNING
```

---

### Lets try it

```
# brew tap linuxkit/linuxkit
# brew install --HEAD linuxkit
```

```
# git clone https://github.com/linuxkit/linuxkit.git
```

```
linuxkit build examples/minimal.yml
linuxkit run minimal
```
  -> Linux machine (Virtual or Baremetal) running containers

---

## RunC

  - control groups
  - namespaces
  - seccomp
  - capabilities
  - apparmor

---

  https://www.docker.com/blog/runc/

  runC is a lightweight, portable container runtime. It includes all of the plumbing code used by Docker to interact with system features related to containers. It is designed with the following principles in mind:

  - Designed for security.
  - Usable at large scale, in production
  - No dependency on the rest of the Docker platform: just the container runtime and nothing else.

---

## ContainerD

- execution - Provide an extensible execution layer for executing a container
- cow filesystem - Built in functionality for overlay, aufs, and other copy on write  filesystems for containers
- distribution - Having the ability to push and pull images as well as operations on images as a first class API object
- metrics - container-level metrics, cgroup stats, and OOM events
- networking - creation and management of network interfaces
- build - Building images as a first class API
- volumes - Volume management for external data
- logging - Persisting container logs

---

## Create hello_world service

```
cat Dockerfile
FROM  alpine:latest
COPY  hello_world.sh /
CMD   [ "/hello_world.sh" ]

```

```
(ns: getty) linuxkit-025000000002:/containers/services/hello_world# ctr -n servi
ces.linuxkit tasks ls
TASK           PID    STATUS
getty          394    RUNNING
hello_world    443    RUNNING
(ns: getty) linuxkit-025000000002:/containers/services/hello_world#
```

---

## passing extra options to virtualmachine

```
linuxkit run hyperkit -mem 2048 -cpus 2 -disk ubuntu.img minimal

```

---

## Links
- https://mobyproject.org/projects/
- https://github.com/buildroot/buildroot
- https://github.com/linuxkit/linuxkit
- https://www.opencontainers.org/
- https://github.com/opencontainers/runc


---


- https://dzone.com/articles/containers-with-out-docker
- http://alokprasad7.blogspot.com/2018/01/fake-i2c-device-i2c-stub.html
- https://www.systutorials.com/docs/linux/man/8-i2c-stub-from-dump/
- https://www.cyberciti.biz/tips/build-linux-kernel-module-against-installed-kernel-source-tree.html
  # load extra modules linuxkit
- https://github.com/linuxkit/linuxkit/issues/2128
- https://github.com/gw0/docker-alpine-kernel-modules
- https://www.ianlewis.org/en/container-runtimes-part-3-high-level-runtimes
  # cross building images
- https://community.arm.com/developer/tools-software/tools/b/tools-software-ides-blog/posts/getting-started-with-docker-for-arm-on-linux
- https://medium.com/@carlosedp/cross-building-arm64-images-on-docker-desktop-254d1e0bc1f9

### other links
  # Namespaces
- https://de.pinout.xyz/pinout/enviro_phat#
- https://medium.com/@tonistiigi/experimenting-with-rootless-docker-416c9ad8c0d6
- https://docs.docker.com/engine/security/userns-remap/
  # Singularity
- https://sylabs.io/guides/3.5/user-guide/installation.html
- http://collabnix.com/installing-docker-18-09-3-on-raspberry-pi-in-2-minutes/
- https://github.com/linuxkit/linuxkit/blob/master/docs/platform-aws.md
- https://collabnix.com/building-linuxkit-os-on-raspberry-pi/
- https://learn.sparkfun.com/tutorials/raspberry-pi-spi-and-i2c-tutorial/all
- https://eraretuya.github.io/2016/12/10/the-i2c-stub-in-action/
- https://github.com/leon-anavi/rpi-examples/blob/master/BMP280/c/BMP280.c
  # wiringPi and alternatives
- https://qiita.com/myasu/items/e3f81b2826ed5797a040
- http://wiringpi.com/wiringpi-deprecated/
- https://packages.ubuntu.com/eoan/libwiringpi2
- https://github.com/containerd/containerd/blob/master/docs/rootless.md
=======
- https://chromium.googlesource.com/external/github.com/docker/containerd/

```

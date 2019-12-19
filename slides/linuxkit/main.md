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

## What is LinuxKit?

  - Open Containers Initiative
  - https://www.opencontainers.org/

---

  - RunC (runtime-spec)
  - ContainerD (image-spec)

---

## RunC

  Linux has a lot of features to enhance security:

  - control groups
  - namespaces
  - seccomp
  - capabilities
  - apparmor

---

  Did you every used them?

---

  https://www.docker.com/blog/runc/

  runC is a lightweight, portable container runtime. It includes all of the plumbing code used by Docker to interact with system features related to containers. It is designed with the following principles in mind:

---

  - Designed for security.
  - Usable at large scale, in production
  - No dependency on the rest of the Docker platform: just the container runtime and nothing else.

---

## ContainerD

- execution - Provide an extensible execution layer for executing a container
- cow filesystem - Built in functionality for overlay, aufs, and other copy on write  filesystems for containers
- networking - creation and management of network interfaces
- distribution - Having the ability to push and pull images as well as operations on images as a first class API object

--

- metrics - container-level metrics, cgroup stats, and OOM events
- build - Building images as a first class API
- volumes - Volume management for external data
- logging - Persisting container logs

---

### LinuxKit config

```
# git clone https://github.com/linuxkit/linuxkit.git

# ls -la linuxkit/examples

# cat linuxkit/examples/minimal.yml

```

---

```
kernel:
  image: linuxkit/kernel:4.19.51
  cmdline: "console=tty0 console=ttyS0 console=ttyAMA0"
init:
  - linuxkit/init:1a0c6b624708b5a9e58b9a608a9ce3164e7b2908
  - linuxkit/runc:c1f0db27e71d948f3134b31ce76276f843849b0a
  - linuxkit/containerd:2e7e59b8af98a1cec834dc9fe7aba271bf4b0a41
...
```

---

```
onboot:
  - name: dhcpcd
    image: linuxkit/dhcpcd:v0.7
    command: ["/sbin/dhcpcd", "--nobackground", "-f", "/dhcpcd.conf", "-1"]
services:
  - name: getty
    image: linuxkit/getty:v0.7
    env:
     - INSECURE=true
trust:
  org:
    - linuxkit
```

---

### Lets try it

```
# brew tap linuxkit/linuxkit
# brew install --HEAD linuxkit
```

```
linuxkit run minimal
```
# linuxkit build examples/minimal.yml
# linuxkit run minimal

  -> Linux machine (Virtual or Baremetal) running containers

---

## Create hello_world service

```
cat Dockerfile
FROM  alpine:latest
COPY  hello_world.sh /
CMD   [ "/hello_world.sh" ]

```

---

```
services:
  - name: hello_world
    image: aturlewicz/hello_world:latest

```

---

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

## Reproducible builds

  docs/reproducible-builds.md

---

## LinuxKit target platforms

- Azure, GCP, AWS, Packet
- RaspberryPi3, Qemu, VMware, VirtualBox, HyperKit

  check docs/ folder for more...

---

  ```


    linuxkit build --format tar examples/minimal.yml
  ```

---

  ```

    linuxkit build --format aws examples/aws.yml
  ```

---

  ```

    linuxkit push aws aws.raw
  ```

---

### RaspberryPi2 is not supported :(

---

  but...

---

### Lets try to run RunC container

---

## Links
- https://mobyproject.org/projects/
- https://github.com/buildroot/buildroot
- https://github.com/linuxkit/linuxkit
- https://www.opencontainers.org/
- https://github.com/opencontainers/runc
- https://chromium.googlesource.com/external/github.com/docker/containerd/
- https://medium.com/faun/using-linuxkit-to-build-an-aws-image-ami-6f73f975b1af
- https://devops.college/linuxkit-on-aws-with-terraform-86786c51d894


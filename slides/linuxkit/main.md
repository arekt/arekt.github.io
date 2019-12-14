## [WIP] LinuxKit - Containers all the way down

Arek Turlewicz
(アレック）

@a_turl

---

## Introduction

  * Linuxkit
  * Use cases
  * Demo

---

## What is LinuxKit?

- Open Containers Initiative
- https://www.opencontainers.org/
- https://github.com/opencontainers/runtime-spec/blob/master/config-linux.md

- RunC (runtime-spec)

- ContainerD (image-spec)

---

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

## Useful staff we can do with RunC

  Creating container content from
  TODO commands to start container
  Example daemon


## Developing IoT projects

Interacting with hardware

It would be nice if we can add some extra modules to the running kernel
(for example emulate I2C device)

Let's say we have project we interact directly on RaspberryPi

Base system was created with Buildroot, but most of the logic is running inside
chroot ubuntu environment.

Problems to solve:

- developers sometimes work remotely, don't want to cary hardware with them
- there is less devices then developers in the company
- CI environment is on remote server (without access to hardware)

---

## commands

```
i2cdetect -y 1
docker build -t load_modules .
docker build -t hello_world  .

```


---

## Links
- https://de.pinout.xyz/pinout/enviro_phat#
- https://github.com/buildroot/buildroot
- https://github.com/linuxkit/linuxkit
- https://www.opencontainers.org/
- https://github.com/opencontainers/runc
- https://dzone.com/articles/containers-with-out-docker
- http://alokprasad7.blogspot.com/2018/01/fake-i2c-device-i2c-stub.html
- https://www.systutorials.com/docs/linux/man/8-i2c-stub-from-dump/
- https://www.cyberciti.biz/tips/build-linux-kernel-module-against-installed-kernel-source-tree.html
  # load extra modules linuxkit
- https://github.com/linuxkit/linuxkit/issues/2128
- https://github.com/gw0/docker-alpine-kernel-modules
- https://www.ianlewis.org/en/container-runtimes-part-3-high-level-runtimes

### other links
  # Namespaces
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

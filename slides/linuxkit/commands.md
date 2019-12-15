
```
 mount --bind /mnt/data/ubu-dev/etc/ssl/certs /etc/ssl/certs
```


```
# mount --bind /mnt/data/ubu-dev/etc/ssl/certs /etc/ssl/certs
# /mnt/data/ubu-dev/usr/local/bin/ctr -n foo images pull docker.io/library/ubuntu:latest
docker.io/library/ubuntu:latest:                                                  resolved       |++++++++++++++++++++++++++++++++++++++| 
index-sha256:6e9f67fa63b0323e9a1e587fd71c561ba48a034504fb804fd26fd8800039835d:    done           |++++++++++++++++++++++++++++++++++++++| 
manifest-sha256:349e3988c0241304b39218794b8263325f7dc517317e00be37d43c3bdda9449b: done           |++++++++++++++++++++++++++++++++++++++| 
layer-sha256:dac0b57271f848c7c02d0795a4f579610e3956c1646254e7809dcdb06c540dbc:    done           |++++++++++++++++++++++++++++++++++++++| 
config-sha256:f576a39bda4498e8e79baae854c04433b19a2412c5ab9d83446b743aab53de7c:   done           |++++++++++++++++++++++++++++++++++++++| 
layer-sha256:60341935f70d34eb6a96ec71514be0b8c977900067664076335f115cfbb73687:    done           |++++++++++++++++++++++++++++++++++++++| 
layer-sha256:3df2b7c380f80ba0f87075cb19d83f61c3b38482aa1dc0893e0e7185471348ac:    done           |++++++++++++++++++++++++++++++++++++++| 
layer-sha256:8c9d3d2adfb541c899c161be416e7ad735dcb5c8516061ace68a953a5d9f57f7:    done           |++++++++++++++++++++++++++++++++++++++| 
elapsed: 163.1s                                                                   total:  21.3 M (133.6 KiB/s)                                     
unpacking linux/arm/v7 sha256:6e9f67fa63b0323e9a1e587fd71c561ba48a034504fb804fd26fd8800039835d...
INFO[0185] apply failure, attempting cleanup             error="failed to extract layer sha256:c664d1b3df75d5193d1d2516d8cec858711fd3d2ae0af22bcd26908d85956761: failed to convert whiteout file \"run/systemd/.wh..wh..opq\": operation not supported: unknown" key="extract-898856380-_-ww sha256:4abf6de5536fd9dd2172e7ec148799748350709c74afff93fcda545cfb62de74"
ctr: failed to extract layer sha256:c664d1b3df75d5193d1d2516d8cec858711fd3d2ae0af22bcd26908d85956761: failed to convert whiteout file "run/systemd/.wh..wh..opq": operation not supported: unknown
# 
```

didn't like overlayfs

```
# /mnt/data/ubu-dev/usr/local/bin/ctr -n foo images pull docker.io/library/redis:alpine
docker.io/library/redis:alpine:                                                   resolved       |++++++++++++++++++++++++++++++++++++++|
index-sha256:ee13953704783b284c080b5b0abe4620730728054f5c19e9488d7a97ecd312c5:    done           |++++++++++++++++++++++++++++++++++++++|
manifest-sha256:7b18da3a57b32a6601d64b22f622aacb9d58b71acdddca19fedafa0692a81ce2: done           |++++++++++++++++++++++++++++++++++++++|
layer-sha256:a394895c0ea179c6d15fb9123554ddc64f0f2009528a05658050c84562f75b63:    done           |++++++++++++++++++++++++++++++++++++++|
layer-sha256:6773b0f0199132543bff53a7e2a21d60520b963aaf6a3127055e7a53c60d6e83:    done           |++++++++++++++++++++++++++++++++++++++|
config-sha256:048d80034387d60575b61563b55ecc92a47bd0f8ecda65fa8c6a22b8763e269d:   done           |++++++++++++++++++++++++++++++++++++++|
layer-sha256:268833695a7b800774f9f43ca8b6a8d5e593ad141625362713624a22a4ea0144:    done           |++++++++++++++++++++++++++++++++++++++|
layer-sha256:99fc70ac0b64db67086f98ceb3942600816eed98046abd6be5ad66f4614a9ca2:    done           |++++++++++++++++++++++++++++++++++++++|
layer-sha256:ee68a0599dc5b5a7923eceeeb11438ca200b59261c8c2d8015596a2c7cf97164:    done           |++++++++++++++++++++++++++++++++++++++|
layer-sha256:00d2426102087e5a411419f35c9ce1a258f79d89957ccaa4143b5b3d448b44f4:    done           |++++++++++++++++++++++++++++++++++++++|
elapsed: 74.7s                                                                    total:  396.7  (5.3 KiB/s)
unpacking linux/arm/v7 sha256:ee13953704783b284c080b5b0abe4620730728054f5c19e9488d7a97ecd312c5...
done
# /mnt/data/ubu-dev/usr/local/bin/ctr -n foo containers create library/redis:alpine r1
ctr: image "library/redis:alpine": not found
# /mnt/data/ubu-dev/usr/local/bin/ctr -n foo containers create docker.io/library/redis:alpine r1
# /mnt/data/ubu-dev/usr/local/bin/ctr -n foo tasks run r1
No help topic for 'run'
# /mnt/data/ubu-dev/usr/local/bin/ctr -n foo tasks start r1
1:C 15 Dec 2019 16:48:53.508 # oO0OoO0OoO0Oo Redis is starting oO0OoO0OoO0Oo
1:C 15 Dec 2019 16:48:53.508 # Redis version=5.0.7, bits=32, commit=00000000, modified=0, pid=1, just started
1:C 15 Dec 2019 16:48:53.508 # Warning: no config file specified, using the default config. In order to specify a config file use redis-server /path/to/redis.conf
1:M 15 Dec 2019 16:48:53.523 # You requested maxclients of 10000 requiring at least 10032 max file descriptors.
1:M 15 Dec 2019 16:48:53.523 # Server can't set maximum open files to 10032 because of OS error: Operation not permitted.
1:M 15 Dec 2019 16:48:53.523 # Current maximum open files is 1024. maxclients has been reduced to 992 to compensate for low ulimit. If you need higher maxclients increase 'ulimit -n'.
1:M 15 Dec 2019 16:48:53.524 # Warning: 32 bit instance detected but no memory limit set. Setting 3 GB maxmemory limit with 'noeviction' policy now.
1:M 15 Dec 2019 16:48:53.527 * Running mode=standalone, port=6379.
1:M 15 Dec 2019 16:48:53.528 # WARNING: The TCP backlog setting of 511 cannot be enforced because /proc/sys/net/core/somaxconn is set to the lower value of 128.
1:M 15 Dec 2019 16:48:53.528 # Server initialized
1:M 15 Dec 2019 16:48:53.528 # WARNING overcommit_memory is set to 0! Background save may fail under low memory condition. To fix this issue add 'vm.overcommit_memory = 1' to /etc/sysctl.conf and then reboot or run the command 'sysctl vm.overcommit_memory=1' for this to take effect.
1:M 15 Dec 2019 16:48:53.529 * Ready to accept connections

```

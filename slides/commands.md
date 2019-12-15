
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

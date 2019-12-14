
# Containerd

`ctr` is commandline client that can connect to containerd daemon through grpc socket 


```
arek@yobo-c930:~/workspace/linuxkit$ sudo ctr namespaces create foo
arek@yobo-c930:~/workspace/linuxkit$ sudo ctr -n foo images pull docker.io/library/redis:alpine
docker.io/library/redis:alpine:                                                   resolved       |++++++++++++++++++++++++++++++++++++++|
index-sha256:ee13953704783b284c080b5b0abe4620730728054f5c19e9488d7a97ecd312c5:    done           |++++++++++++++++++++++++++++++++++++++|
manifest-sha256:ae9dd3bbe42bf13bc318af4af2842b323465312392b96d44893895e8a0438565: done           |++++++++++++++++++++++++++++++++++++++|
layer-sha256:60027bdc030cbd93c908afd40e3ff420f18c77247e59753e57427263bdc84ef5:    done           |++++++++++++++++++++++++++++++++++++++|
layer-sha256:b2eb22a0b7db31a2b1e2b105bf445ef69f2b80a0957cc66d9d27ca32ef9dc8e8:    done           |++++++++++++++++++++++++++++++++++++++|
layer-sha256:c5ccbdf10203a49131c170f17a9aea9fed5b9b13b745ffbdb92e31586804050f:    done           |++++++++++++++++++++++++++++++++++++++|
config-sha256:a49ff3e0d85f0b60ddf225db3c134ed1735a3385d9cc617457b21875673da2f0:   done           |++++++++++++++++++++++++++++++++++++++|
layer-sha256:592c37d15428bbf75740a87ea79d97e07aac0e7945ff2c2c9f191d3cb0572982:    done           |++++++++++++++++++++++++++++++++++++++|
layer-sha256:b70a614994bf61cd50e30f4a5539943ea7839d5508434bd5fcf734179bb4f990:    done           |++++++++++++++++++++++++++++++++++++++|
layer-sha256:89d9c30c1d48bac627e5c6cb0d1ed1eec28e7dbdfbcc04712e4c79c0f83faf17:    done           |++++++++++++++++++++++++++++++++++++++|
elapsed: 41.4s                                                                    total:  9.4 Mi (233.3 KiB/s)
unpacking linux/amd64 sha256:ee13953704783b284c080b5b0abe4620730728054f5c19e9488d7a97ecd312c5...
done
```

If you don't like command line, you can use go to programmatically interact with containerd.
Check this out: https://containerd.io/docs/getting-started/

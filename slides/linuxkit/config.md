
```
# cat /mnt/data/ubu-dev/etc/containerd/config.toml
subreaper = true
oom_score = -999

root = "/mnt/data/containerd"

[debug]
        level = "debug"

[metrics]
        address = "127.0.0.1:1338"

[plugins.linux]
        runtime = "/mnt/data/ubu-dev/usr/local/sbin/runc"
        shim_debug = true
# 
```

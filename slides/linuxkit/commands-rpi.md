root@malinka:/# ctr namespaces ls
NAME LABELS 
foo         
root@malinka:/# ctr foo images   
No help topic for 'foo'
root@malinka:/# ctr -n foo images
NAME:
   ctr images - manage images

USAGE:
   ctr images command [command options] [arguments...]

COMMANDS:
   check       check that an image has all content available locally
   export      export images
   import      import images
   list, ls    list images known to containerd
   pull        pull an image from a remote
   push        push an image to a remote
   remove, rm  remove one or more images by reference
   tag         tag an image
   label       set and clear labels for an image

OPTIONS:
   --help, -h  show help
   
root@malinka:/# ctr -n foo images
NAME:
   ctr images - manage images

USAGE:
   ctr images command [command options] [arguments...]

COMMANDS:
   check       check that an image has all content available locally
   export      export images
   import      import images
   list, ls    list images known to containerd
   pull        pull an image from a remote
   push        push an image to a remote
   remove, rm  remove one or more images by reference
   tag         tag an image
   label       set and clear labels for an image

OPTIONS:
   --help, -h  show help
   
root@malinka:/# ctr -n foo images list
REF TYPE DIGEST SIZE PLATFORMS LABELS 
root@malinka:/# ctr -n foo images pull redis:alpine
redis:alpine:  resolving      |--------------------------------------| 
elapsed: 0.1 s total:   0.0 B (0.0 B/s)                                         
ctr: failed to resolve reference "redis:alpine": object required
root@malinka:/# ctr -n foo images pull library/redis:alpine
library/redis:alpine: resolving      |--------------------------------------| 
elapsed: 0.2 s        total:   0.0 B (0.0 B/s)                                         
ctr: failed to resolve reference "library/redis:alpine": failed to do request: Head https://library/v2/redis/manifests/alpine: dial tcp: lookup library on 1.1.1.1:53: no such host
root@malinka:/# ifconfig
bash: ifconfig: command not found
root@malinka:/# ctr -n foo images pull library/redis:alpine
library/redis:alpine: resolving      |--------------------------------------| 
elapsed: 0.1 s        total:   0.0 B (0.0 B/s)                                         
ctr: failed to resolve reference "library/redis:alpine": failed to do request: Head https://library/v2/redis/manifests/alpine: dial tcp: lookup library on 1.1.1.1:53: no such host
root@malinka:/# ctr -n foo images pull redis:alpine
ctr: failed to resolve reference "redis:alpine": object required
root@malinka:/# ctr -n foo containers start hello /containers/services/
getty/ hello/ 
root@malinka:/# ctr -n foo containers start hello /containers/services/hello 
No help topic for 'start'
root@malinka:/# ctr -n foo containers create hello /containers/services/hello
ctr: image "hello": not found
root@malinka:/# ctr help
NAME:
   ctr - 
        __
  _____/ /______
 / ___/ __/ ___/
/ /__/ /_/ /
\___/\__/_/

containerd CLI


USAGE:
   ctr [global options] command [command options] [arguments...]

VERSION:
   v1.3.0-176-g54736371

DESCRIPTION:
   
ctr is an unsupported debug and administrative client for interacting
with the containerd daemon. Because it is unsupported, the commands,
options, and operations are not guaranteed to be backward compatible or
stable from release to release of the containerd project.

COMMANDS:
   plugins, plugin            provides information about containerd plugins
   version                    print the client and server versions
   containers, c, container   manage containers
   content                    manage content
   events, event              display containerd events
   images, image, i           manage images
   leases                     manage leases
   namespaces, namespace, ns  manage namespaces
   pprof                      provide golang pprof outputs for containerd
   run                        run a container
   snapshots, snapshot        manage snapshots
   tasks, t, task             manage tasks
   install                    install a new package
   shim                       interact with a shim directly
   help, h                    Shows a list of commands or help for one command

GLOBAL OPTIONS:
   --debug                      enable debug output in logs
   --address value, -a value    address for containerd's GRPC server (default: "/run/containerd/containerd.sock")
   --timeout value              total timeout for ctr commands (default: 0s)
   --connect-timeout value      timeout for connecting to containerd (default: 0s)
   --namespace value, -n value  namespace to use with commands (default: "default") [$CONTAINERD_NAMESPACE]
   --help, -h                   show help
   --version, -v                print the version
root@malinka:/# ctr containers
NAME:
   ctr containers - manage containers

USAGE:
   ctr containers command [command options] [arguments...]

COMMANDS:
   create           create container
   delete, del, rm  delete one or more existing containers
   info             get info about a container
   list, ls         list containers
   label            set and clear labels for a container
   checkpoint       checkpoint a container
   restore          restore a container from checkpoint

OPTIONS:
   --help, -h  show help
   
root@malinka:/# ctr container 
NAME:
   ctr containers - manage containers

USAGE:
   ctr containers command [command options] [arguments...]

COMMANDS:
   create           create container
   delete, del, rm  delete one or more existing containers
   info             get info about a container
   list, ls         list containers
   label            set and clear labels for a container
   checkpoint       checkpoint a container
   restore          restore a container from checkpoint

OPTIONS:
   --help, -h  show help
   
root@malinka:/# ctr -n foo containers create --help                          
NAME:
   ctr containers create - create container

USAGE:
   ctr containers create [command options] [flags] Image|RootFS CONTAINER [COMMAND] [ARG...]

OPTIONS:
   --snapshotter value       snapshotter name. Empty value stands for the default value. [$CONTAINERD_SNAPSHOTTER]
   --config value, -c value  path to the runtime-specific spec config file
   --cwd value               specify the working directory of the process
   --env value               specify additional container environment variables (i.e. FOO=bar)
   --env-file value          specify additional container environment variables in a file(i.e. FOO=bar, one per line)
   --label value             specify additional labels (i.e. foo=bar)
   --mount value             specify additional container mount (ex: type=bind,src=/tmp,dst=/host,options=rbind:ro)
   --net-host                enable host networking for the container
   --privileged              run privileged container
   --read-only               set the containers filesystem as readonly
   --runtime value           runtime name (default: "io.containerd.runc.v2")
   --tty, -t                 allocate a TTY for the container
   --with-ns value           specify existing Linux namespaces to join at container runtime (format '<nstype>:<path>')
   --pid-file value          file path to write the task's pid
   --gpus value              add gpus to the container (default: 0)
   --allow-new-privs         turn off OCI spec's NoNewPrivileges feature flag
   --memory-limit value      memory limit (in bytes) for the container (default: 0)
   --device value            add a device to a container
   --seccomp                 enable the default seccomp profile
   --rootfs                  use custom rootfs that is not managed by containerd snapshotter
   --no-pivot                disable use of pivot-root (linux only)
   
root@malinka:/# ctr -n foo containers create -c /containers/services/hello/
config.json   lower/        rootfs/       runtime.json  tmp/          
root@malinka:/# ctr -n foo containers create -c /containers/services/hello/config.json 
ctr: container id must be provided: invalid argument
root@malinka:/# ctr -n foo containers create -c /containers/services/hello/config.json h1
root@malinka:/# ctr -n foo containers list
CONTAINER    IMAGE    RUNTIME                  
h1           -        io.containerd.runc.v2    
root@malinka:/# ctr -n foo containers run h1
No help topic for 'run'
root@malinka:/# ctr -n foo containers --help
NAME:
   ctr containers - manage containers

USAGE:
   ctr containers command [command options] [arguments...]

COMMANDS:
   create           create container
   delete, del, rm  delete one or more existing containers
   info             get info about a container
   list, ls         list containers
   label            set and clear labels for a container
   checkpoint       checkpoint a container
   restore          restore a container from checkpoint

OPTIONS:
   --help, -h  show help
   
root@malinka:/# ctr -n foo containers run -t sh
No help topic for 'run'
root@malinka:/# pwd
/
root@malinka:/# ls -la
total 68
drwxr-xr-x  21 1000 1000 4096 Dec 15 11:59 .
drwxr-xr-x  21 1000 1000 4096 Dec 15 11:59 ..
-rwxr-xr-x   1 1000 1000    0 Dec  9 11:38 .dockerenv
drwxr-xr-x   2 1000 1000 4096 Dec 15 10:25 bin
drwxr-xr-x   2 1000 1000 4096 Apr 24  2018 boot
drwxr-xr-x   4 root root   57 Apr 18  2017 containers
drwxr-xr-x  10 root root 2560 Jan  1  1970 dev
drwxr-xr-x  74 1000 1000 4096 Dec 15 10:31 etc
drwxr-xr-x   2 1000 1000 4096 Apr 24  2018 home
drwxr-xr-x  12 1000 1000 4096 Dec 12 07:57 lib
drwxr-xr-x   2 1000 1000 4096 Dec  9 11:11 media
drwxr-xr-x   2 1000 1000 4096 Dec  9 11:11 mnt
drwxr-xr-x   3 1000 1000 4096 Dec 15 10:55 opt
dr-xr-xr-x 103 root root    0 Jan  1  1970 proc
drwx------  11 1000 1000 4096 Dec 15 13:18 root
drwxr-xr-x   7 1000 1000 4096 Dec 14 08:57 run
drwxr-xr-x   2 1000 1000 4096 Dec 12 07:58 sbin
drwxr-xr-x   2 1000 1000 4096 Dec  9 11:11 srv
drwxr-xr-x   2 1000 1000 4096 Apr 24  2018 sys
drwxrwxrwt   5 root root  200 Dec 15 11:57 tmp
drwxr-xr-x  10 1000 1000 4096 Dec  9 11:11 usr
drwxr-xr-x  11 1000 1000 4096 Dec  9 11:11 var
root@malinka:/# exit
exit
# bash    
root@malinka:/# ctr -n foo containers list     
CONTAINER    IMAGE    RUNTIME                  
h1           -        io.containerd.runc.v2    
root@malinka:/# ctr -n foo containers rm h1
root@malinka:/# ctr -n foo containers list
CONTAINER    IMAGE    RUNTIME    
root@malinka:/# ctr -n foo containers create -c /containers/services/hello/config.json --rootfs /containers/services/hello/rootfs h1
ctr: with spec config file, only container id should be provided: invalid argument
root@malinka:/# ctr -n foo containers run -c /containers/services/hello/config.json h1                                           
No help topic for 'run'
root@malinka:/# ctr -n foo containers create -c /containers/services/hello/config.json h1
root@malinka:/# ctr -n foo containers create -c /containers/services/hello/config.json h1
ctr: container "h1": already exists
root@malinka:/# ctr -n foo task help
NAME:
   ctr tasks - manage tasks

USAGE:
   ctr tasks [global options] command [command options] [arguments...]

VERSION:
   v1.3.0-176-g54736371

COMMANDS:
   attach           attach to the IO of a running container
   checkpoint       checkpoint a container
   delete, rm       delete one or more tasks
   exec             execute additional processes in an existing container
   list, ls         list tasks
   kill             signal a container (default: SIGTERM)
   pause            pause an existing container
   ps               list processes for container
   resume           resume a paused container
   start            start a container that has been created
   metrics, metric  get a single data point of metrics for a task with the built-in Linux runtime

GLOBAL OPTIONS:
   --help, -h  show help
root@malinka:/# ctr -n foo task list
TASK    PID    STATUS    
root@malinka:/# ctr -n foo task start h1
ctr: OCI runtime create failed: container_linux.go:265: starting container process caused "process_linux.go:264: applying cgroup configuration for process caused \"mountpoint for cgroup not found\"": unknown
root@malinka:/# exit
exit
# exit
# vi /mnt/data/
lost+found/                 motion-detect.sh            ro.sh                       ubu-dev-chroot.sh           ubu-dev/
minimal-armhf-squashfs.img  nohup.out                   rw.sh                       ubu-dev.tar.gz              ubuntu-armhf/
# vi /mnt/data/ubu-dev-chroot.sh 
# ./ubu-dev-chroot.sh
# bash
root@malinka:/# ctr -n foo task start h1
ctr: OCI runtime create failed: container_linux.go:265: starting container process caused "process_linux.go:264: applying cgroup configuration for process caused \"mountpoint for cgroup not found\"": unknown
root@malinka:/# ls -la /proc/
Display all 150 possibilities? (y or n)
root@malinka:/# ls -la /proc/cgroups 
.dockerenv  boot/       dev/        home/       media/      opt/        root/       sbin/       sys/        usr/        
bin/        containers/ etc/        lib/        mnt/        proc/       run/        srv/        tmp/        var/        
root@malinka:/# ls -la /proc/cgroups 
-r--r--r-- 1 root root 0 Dec 15 13:42 /proc/cgroups
root@malinka:/# exit
exit
# exit
# ls -la /proc/cgroups 
-r--r--r--    1 root     root             0 Dec 15 13:42 /proc/cgroups
# mount /proc/cgroups 
mount: can't find /proc/cgroups in /etc/fstab
# mount -t cgroup -o all cgroup /sys/fs/cgroup
mount: mounting cgroup on /sys/fs/cgroup failed: Device or resource busy
# ls -la /sys/fs/cgroup/
total 0
dr-xr-xr-x    2 root     root             0 Jan  1  1970 .
drwxr-xr-x    5 root     root             0 Jan  1  1970 ..
-r--r--r--    1 root     root             0 Dec 15 13:44 blkio.io_merged
-r--r--r--    1 root     root             0 Dec 15 13:44 blkio.io_merged_recursive
-r--r--r--    1 root     root             0 Dec 15 13:44 blkio.io_queued
-r--r--r--    1 root     root             0 Dec 15 13:44 blkio.io_queued_recursive
-r--r--r--    1 root     root             0 Dec 15 13:44 blkio.io_service_bytes
-r--r--r--    1 root     root             0 Dec 15 13:44 blkio.io_service_bytes_recursive
-r--r--r--    1 root     root             0 Dec 15 13:44 blkio.io_service_time
-r--r--r--    1 root     root             0 Dec 15 13:44 blkio.io_service_time_recursive
-r--r--r--    1 root     root             0 Dec 15 13:44 blkio.io_serviced
-r--r--r--    1 root     root             0 Dec 15 13:44 blkio.io_serviced_recursive
-r--r--r--    1 root     root             0 Dec 15 13:44 blkio.io_wait_time
-r--r--r--    1 root     root             0 Dec 15 13:44 blkio.io_wait_time_recursive
-rw-r--r--    1 root     root             0 Dec 15 13:44 blkio.leaf_weight
-rw-r--r--    1 root     root             0 Dec 15 13:44 blkio.leaf_weight_device
--w-------    1 root     root             0 Dec 15 13:44 blkio.reset_stats
-r--r--r--    1 root     root             0 Dec 15 13:44 blkio.sectors
-r--r--r--    1 root     root             0 Dec 15 13:44 blkio.sectors_recursive
-r--r--r--    1 root     root             0 Dec 15 13:44 blkio.throttle.io_service_bytes
-r--r--r--    1 root     root             0 Dec 15 13:44 blkio.throttle.io_serviced
-rw-r--r--    1 root     root             0 Dec 15 13:44 blkio.throttle.read_bps_device
-rw-r--r--    1 root     root             0 Dec 15 13:44 blkio.throttle.read_iops_device
-rw-r--r--    1 root     root             0 Dec 15 13:44 blkio.throttle.write_bps_device
-rw-r--r--    1 root     root             0 Dec 15 13:44 blkio.throttle.write_iops_device
-r--r--r--    1 root     root             0 Dec 15 13:44 blkio.time
-r--r--r--    1 root     root             0 Dec 15 13:44 blkio.time_recursive
-rw-r--r--    1 root     root             0 Dec 15 13:44 blkio.weight
-rw-r--r--    1 root     root             0 Dec 15 13:44 blkio.weight_device
-rw-r--r--    1 root     root             0 Dec 15 13:44 cgroup.clone_children
-rw-r--r--    1 root     root             0 Dec 15 13:44 cgroup.procs
-r--r--r--    1 root     root             0 Dec 15 13:44 cgroup.sane_behavior
-rw-r--r--    1 root     root             0 Dec 15 13:44 cpu.shares
-r--r--r--    1 root     root             0 Dec 15 13:44 cpuacct.stat
-rw-r--r--    1 root     root             0 Dec 15 13:44 cpuacct.usage
-r--r--r--    1 root     root             0 Dec 15 13:44 cpuacct.usage_all
-r--r--r--    1 root     root             0 Dec 15 13:44 cpuacct.usage_percpu
-r--r--r--    1 root     root             0 Dec 15 13:44 cpuacct.usage_percpu_sys
-r--r--r--    1 root     root             0 Dec 15 13:44 cpuacct.usage_percpu_user
-r--r--r--    1 root     root             0 Dec 15 13:44 cpuacct.usage_sys
-r--r--r--    1 root     root             0 Dec 15 13:44 cpuacct.usage_user
-rw-r--r--    1 root     root             0 Dec 15 13:44 cpuset.cpu_exclusive
-rw-r--r--    1 root     root             0 Dec 15 13:44 cpuset.cpus
-r--r--r--    1 root     root             0 Dec 15 13:44 cpuset.effective_cpus
-r--r--r--    1 root     root             0 Dec 15 13:44 cpuset.effective_mems
-rw-r--r--    1 root     root             0 Dec 15 13:44 cpuset.mem_exclusive
-rw-r--r--    1 root     root             0 Dec 15 13:44 cpuset.mem_hardwall
-rw-r--r--    1 root     root             0 Dec 15 13:44 cpuset.memory_migrate
-r--r--r--    1 root     root             0 Dec 15 13:44 cpuset.memory_pressure
-rw-r--r--    1 root     root             0 Dec 15 13:44 cpuset.memory_pressure_enabled
-rw-r--r--    1 root     root             0 Dec 15 13:44 cpuset.memory_spread_page
-rw-r--r--    1 root     root             0 Dec 15 13:44 cpuset.memory_spread_slab
-rw-r--r--    1 root     root             0 Dec 15 13:44 cpuset.mems
-rw-r--r--    1 root     root             0 Dec 15 13:44 cpuset.sched_load_balance
-rw-r--r--    1 root     root             0 Dec 15 13:44 cpuset.sched_relax_domain_level
--w-------    1 root     root             0 Dec 15 13:44 devices.allow
--w-------    1 root     root             0 Dec 15 13:44 devices.deny
-r--r--r--    1 root     root             0 Dec 15 13:44 devices.list
-rw-r--r--    1 root     root             0 Dec 15 13:44 net_cls.classid
-rw-r--r--    1 root     root             0 Dec 15 13:44 notify_on_release
-rw-r--r--    1 root     root             0 Dec 15 13:44 release_agent
-rw-r--r--    1 root     root             0 Dec 15 13:44 tasks
# vi /mnt/data/ubu-dev-chroot.sh 
# vi /mnt/data/ubu-dev-chroot.sh 
# mount --bind /sys /mnt/data/ubu-dev/sys
# ./ubu-dev-chroot.sh
# bash
root@malinka:/# ctr -n foo task start h1
ctr: OCI runtime create failed: container_linux.go:265: starting container process caused "process_linux.go:264: applying cgroup configuration for process caused \"mountpoint for cgroup not found\"": unknown
root@malinka:/# ls -la /sys/
block/    bus/      class/    dev/      devices/  firmware/ fs/       kernel/   module/   power/    
root@malinka:/# ls -la /sys/fs/cgroup/
total 0
dr-xr-xr-x 2 root root 0 Jan  1  1970 .
drwxr-xr-x 5 root root 0 Jan  1  1970 ..
root@malinka:/# mount -t cgroup -o all cgroup /sys/fs/cgroup
root@malinka:/# ctr -n foo task start h1
ctr: OCI runtime create failed: container_linux.go:265: starting container process caused "process_linux.go:348: container init caused \"rootfs_linux.go:45: preparing rootfs caused \\\"invalid argument\\\"\"": unknown
root@malinka:/# ctr -n foo task start --help
NAME:
   ctr tasks start - start a container that has been created

USAGE:
   ctr tasks start [command options] CONTAINER

OPTIONS:
   --null-io         send all IO to /dev/null
   --log-uri value   log uri
   --fifo-dir value  directory used for storing IO FIFOs
   --pid-file value  file path to write the task's pid
   --detach, -d      detach from the task after it has started execution
   
root@malinka:/# ctr -n foo containers rm h1
root@malinka:/# ctr -n foo containers create --rootfs /containers/services/hello h1
root@malinka:/# ctr -n foo task start h1
ctr: OCI runtime create failed: args must not be empty: unknown
root@malinka:/# ctr -n foo task start h1 date 
ctr: OCI runtime create failed: args must not be empty: unknown
root@malinka:/# ctr -n foo task start --help 
NAME:
   ctr tasks start - start a container that has been created

USAGE:
   ctr tasks start [command options] CONTAINER

OPTIONS:
   --null-io         send all IO to /dev/null
   --log-uri value   log uri
   --fifo-dir value  directory used for storing IO FIFOs
   --pid-file value  file path to write the task's pid
   --detach, -d      detach from the task after it has started execution
   
root@malinka:/# ctr -n foo containers rm h1
root@malinka:/# ctr -n foo containers create --rootfs /containers/services/hello/rootfs h1
root@malinka:/# ctr -n foo task start --help
NAME:
   ctr tasks start - start a container that has been created

USAGE:
   ctr tasks start [command options] CONTAINER

OPTIONS:
   --null-io         send all IO to /dev/null
   --log-uri value   log uri
   --fifo-dir value  directory used for storing IO FIFOs
   --pid-file value  file path to write the task's pid
   --detach, -d      detach from the task after it has started execution
   
root@malinka:/# ctr -n foo task start h1    
ctr: OCI runtime create failed: args must not be empty: unknown
root@malinka:/# ctr -n foo task start h1 data
ctr: OCI runtime create failed: args must not be empty: unknown
root@malinka:/# ctr -n foo task start h1 date
ctr: OCI runtime create failed: args must not be empty: unknown
root@malinka:/# ctr -n foo task start /bin/date h1
ctr: container "/bin/date" in namespace "foo": not found
root@malinka:/# ctr -n foo containers create -c /containers/services/hello/config.json h1^C
root@malinka:/# ctr -n foo containers rm h1
root@malinka:/# ctr -n foo containers create --rootfs /containers/services/hello/rootfs -c /containers/services/hello/config.json h1
ctr: create container failed validation: container.ID: identifier "-c" must match ^[A-Za-z0-9]+(?:[._-](?:[A-Za-z0-9]+))*$: invalid argument
root@malinka:/# ctr -n foo containers create --rootfs /containers/services/hello/rootfs h1 -c /containers/services/hello/config.json   
ctr: create container failed validation: container.ID: identifier "-c" must match ^[A-Za-z0-9]+(?:[._-](?:[A-Za-z0-9]+))*$: invalid argument
root@malinka:/# ctr -n foo containers create --rootfs /containers/services/hello/rootfs h1 /bin/date                                
root@malinka:/# ctr -n foo task start h1
ctr: OCI runtime create failed: container_linux.go:265: starting container process caused "process_linux.go:348: container init caused \"rootfs_linux.go:45: preparing rootfs caused \\\"invalid argument\\\"\"": unknown
root@malinka:/# 


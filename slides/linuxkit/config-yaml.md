

### Yaml Specification

The yaml format specifies the image to be built:

- `kernel` specifies a kernel Docker image, containing a kernel and a filesystem tarball, eg containing modules. The example kernels are built from `kernel/`
- `init` is the base `init` process Docker image, which is unpacked as the base system, containing `init`, `containerd`, `runc` and a few tools. Built from `pkg/init/`
- `onboot` are the system containers, executed sequentially in order. They should terminate quickly when done.
- `services` is the system services, which normally run for the whole time the system is up
- `files` are additional files to add to the image


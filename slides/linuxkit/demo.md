cd /tmp
mkdir hello
cd hello
cp -a /mnt/data/containers/hello/lower rootfs
runc spec
runc run h1

# **I MADE DRIVER FOR UTM. USE THIS [REPO LINK](https://github.com/iIIusi0n/docker-machine-driver-utm/tree/main)**

# Docker machine qemu driver for latest version of MacOS

I needed a non-libvirt qemu driver, so this is it.

It worked in M1 Mac already but with `user` networking mode only.
But I needed to create seperated network topology for guest instances.
This is edited driver that support vmnet-*. (It requires sudo privileges)

from @SvenDowideit

Its initial use is going to be for running the [Rancher OS](https://github.com/rancher/os) tests, but maybe you'll find a use for it too.


from @fventuri

#### QEMU

Create machines locally using [QEMU](http://www.qemu.org/).
This driver requires QEMU to be installed on your host.

    $ docker-machine create --driver=qemu qemu-test

Options:

 - `--qemu-boot2docker-url`: The URL of the boot2docker image. Defaults to the latest available version.
 - `--qemu-disk-size`: Size of disk for the host in MB. Default: `20000`
 - `--qemu-memory`: Size of memory for the host in MB. Default: `1024`
 - `--qemu-cpu-count`: Number of CPUs. Default: `1`
 - `--qemu-program` : Name of the qemu program to run. Default: `qemu-system-x86_64`
 - `--qemu-display` : Show the graphical display output to the user. Default: false
 - `--qemu-display-type` : Select type of display to use (sdl/vnc=localhost:0/etc)
 - `--qemu-nographic` : Use -nographic instead of -display none. Default: false
 - `--qemu-virtio-drives` : Use virtio for drives (cdrom and disk). Default: false
 - `--qemu-network`: Networking to be used: vmnet-shared, vmnet-host or vmnet-bridged. Default: `vmnet-shared`
 - `--qemu-network-bridge`: Name of the network bridge to be used for networking. (for vmnet-bridged) Default: `en0`
 - `--qemu-network-vmnet-id`: The vmnet id to use for networking. Default: `net0`

The `--qemu-boot2docker-url` flag takes a few different forms.  By
default, if no value is specified for this flag, Machine will check locally for
a boot2docker ISO.  If one is found, that will be used as the ISO for the
created machine.  If one is not found, the latest ISO release available on
[boot2docker/boot2docker](https://github.com/boot2docker/boot2docker) will be
downloaded and stored locally for future use.  Note that this means you must run
`docker-machine upgrade` deliberately on a machine if you wish to update the "cached"
boot2docker ISO.

This is the default behavior (when `--qemu-boot2docker-url=""`), but the
option also supports specifying ISOs by the `http://` and `file://` protocols.
`file://` will look at the path specified locally to locate the ISO: for
instance, you could specify `--qemu-boot2docker-url
file://$HOME/Downloads/rc.iso` to test out a release candidate ISO that you have
downloaded already.  You could also just get an ISO straight from the Internet
using the `http://` form.

Note that when using virtio the drives will be mounted as `/dev/vda` and `/dev/vdb`,
instead of the usual `/dev/cdrom` and `/dev/sda`, since they are using paravirtualization.

Environment variables:

Here comes the list of the supported variables with the corresponding options. If both environment
variable and CLI option are provided the CLI option takes the precedence.

| Environment variable              | CLI option                        |
|-----------------------------------|-----------------------------------|
| `QEMU_BOOT2DOCKER_URL`            | `--qemu-boot2docker-url`          |
| `QEMU_VIRTIO_DRIVES`              | `--qemu-virtio-drives`            |

#### WARNING!

It strongly depends on `arp -a` command to resolve IP from Mac address. 
Not tested on other systems, it assumes output of `arp -a` will be list of `? <IP> <MAC>`.

UPDATED: It is updated to read and parse DHCP leases file but still not tested on various systems.

Planning to use QMP to solve it.

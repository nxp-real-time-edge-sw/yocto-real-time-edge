# Real-time Edge Software Manifest README

## Install the `repo` utility (only need to do this once)

To get the Real-time Edge environment you need to have `repo` installed.

This 'repo' is used to download manifests for Real-time Edge releases.

```
$ mkdir ~/bin
$ curl https://storage.googleapis.com/git-repo-downloads/repo  > ~/bin/repo
$ chmod a+x ~/bin/repo
$ export PATH=${PATH}:~/bin
```

## Download the latest Real-time Edge Environment

```
$ mkdir yocto-rt-edge
$ cd yocto-rt-edge
$ repo init -u https://github.com/rt-edge-sw/yocto-rt-edge.git -m default.xml

$ repo sync
```

## Download a specific release of Real-time Edge Environment

The corresponding branch should be used when downloading a specific release.

Please refer to detailed README under the release branch.

### Examples

To download the Real-time Edge 2.0 release

```
$ mkdir yocto-rt-edge
$ cd yocto-rt-edge
$ repo init -u https://github.com/rt-edge-sw/yocto-rt-edge.git -b rt-edge-gatesgarth -m rt-edge-2.0.0.xml
```

## Setup build project

```
$ MACHINE=<Machine> DISTRO=<Distro> source ./rt-edge-setup-env.sh -b bld-<Name>
```

Machine:
- imx6ull14x14evk
- imx8mmevk
- imx8mpevk
- ls1028ardb
- ls1012ardb
- ls1021atwr
- ls1021atsn
- ls1021aiot
- ls1043ardb
- ls1046ardb
- ls1046afrwy
- lx2160ardb
- lx2160ardb-rev2

Distro:
- nxp-rt-edge – The regular image including Real-time Networking, Real-time System, and Industrial packages.
- nxp-rt-edge-baremetal – The baremetal image(some boards do not support this distro).

Name:
- identical string for the build project

### Examples

```
$ DISTRO=nxp-rt-edge-baremetal MACHINE=imx8mpevk source rt-edge-setup-env.sh -b build-imx8mpevk-baremetal
```

```
$ DISTRO=nxp-rt-edge MACHINE=imx8mpevk source rt-edge-setup-env.sh -b build-imx8mpevk-rt-edge
```

## Build an image

```
$ bitbake <Image>
```

Image:
- nxp-image-rt-edge: demo image for all supported machines.

### Examples

```
$ bitbake nxp-image-rt-edge
```

# Real-time Edge Software Manifest

## Install the `repo` utility (only need to do this once)

To get the Real-time Edge environment you need to have `repo` installed (see [link](https://gerrit.googlesource.com/git-repo/+/refs/heads/master/README.md#install) or instructions below).  
This `repo` package is used to download manifests for Real-time Edge releases.  
This can be installed with your package manager or manually.

### Install Repo with Package Manager
```
# Debian/Ubuntu.
$ sudo apt-get install repo

# Gentoo.
$ sudo emerge dev-vcs/repo
```
### Install Repo manually
```
$ mkdir -p ~/.bin
$ PATH="${HOME}/.bin:${PATH}"
$ curl https://storage.googleapis.com/git-repo-downloads/repo > ~/.bin/repo
$ chmod a+rx ~/.bin/repo
```

## Download the latest Real-time Edge Environment

```
$ mkdir yocto-real-time-edge
$ cd yocto-real-time-edge
$ repo init -u https://github.com/real-time-edge-sw/yocto-real-time-edge.git -m default.xml

$ repo sync
```

## Download a specific release of Real-time Edge Environment

The corresponding branch should be used when downloading a specific release.

Please refer to detailed README under the release branch.

### Examples

To download the Real-time Edge 2.0 release

```
$ mkdir yocto-real-time-edge
$ cd yocto-real-time-edge
$ repo init -u https://github.com/real-time-edge-sw/yocto-real-time-edge.git -b real-time-edge-hardknott -m default.xml
```

## Setup build project

```
$ MACHINE=<Machine> DISTRO=<Distro> source ./real-time-edge-setup-env.sh -b bld-<Name>
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
- nxp-real-time-edge – The regular image including Real-time Networking, Real-time System, and Industrial packages.
- nxp-real-time-edge-baremetal – The baremetal image(some boards do not support this distro).

Name:
- identical string for the build project

### Examples

```
$ DISTRO=nxp-real-time-edge-baremetal MACHINE=imx8mpevk source real-time-edge-setup-env.sh -b build-imx8mpevk-baremetal
```

```
$ DISTRO=nxp-real-time-edge MACHINE=imx8mpevk source real-time-edge-setup-env.sh -b build-imx8mpevk-real-time-edge
```

## Build an image

```
$ bitbake <Image>
```

Image:
- nxp-image-real-time-edge: demo image for all supported machines.

### Examples

```
$ bitbake nxp-image-real-time-edge
```

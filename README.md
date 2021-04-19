Yocto Project BSP Releases
=======================================

To get the BSP you need to have `repo` installed.

Install the `repo` utility: (only need to do this once):
--------------------------------------------------
```
$ mkdir ~/bin
$ curl https://storage.googleapis.com/git-repo-downloads/repo  > ~/bin/repo
$ chmod a+x ~/bin/repo
$ export PATH=${PATH}:~/bin
```

Setup key in bitbucket
----------------------------------------------
- The Yocto recipes have been updated to require authentication for access to repositories on the new NXP Bitbucket server
- You must now have matching SSH keys on your local build machine and in your Bitbucket account profile in order to run builds

- If your local machine account already has SSH keys,
then copy-paste the contents of the ~/.ssh/id_rsa.pub file into the SSH Keys section
of your Bitbucket account profile (go to "Manage account"
from the user menu in the upper right of the Bitbucket Web UI toolbar)

- If you do not already have an id_rsa.pub file, then you need to generate a new SSH key pair
using the following command: ssh-keygen -t rsa -b 2048
(if prompted to enter a passphrase, click enter to leave it blank)

- Users who have never accessed Bitbucket using the Web UI may be denied access
due to a temporary licensing issue with the new servers; if this affects you,
submit a ticket to nxp2.service-now.com to get IT assistance activating your Bitbucket account

- You may receive an error trying to fetch a new repository. In this case, try
to manually access the repository and/or its mirror(s). If you see a prompt
regarding the authenticity of the host, accept it, and the problem should be fixed.


Download the BSP Yocto Project Environment into your directory:
-------------------------------------------
```
$ mkdir yocto-rt-edge
$ cd yocto-rt-edge
$ repo init -u ssh://git@bitbucket.sw.nxp.com/dnind/yocto-rt-edge.git -m default.xml

$ repo sync
```

Setup build project:
-----------------------------------------
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

Distro:
- imx-rt-edge: real time edge image for i.MX
- imx-baremetal: real time edge image with baremetal binary for i.MX
- qoriq-rt-edge: real time edge image for Layerscape
- qoriq-baremetal: real time edge image with baremetal binary for Layerscape

Name:
- identical string for the build project

For example:

```
$ DISTRO=imx-baremetal MACHINE=imx8mpevk source rt-edge-setup-env.sh -b build-imx8mpevk-baremetal
```

```
$ DISTRO=qoriq-rt-edge MACHINE=ls1043ardb source rt-edge-setup-env.sh -b build-ls1043ardb-rt-edge
```

Build an image:
---------------
```
$ bitbake <image>
```

Some images:

imx-image-rt-edge: demo image for i.MX machines
qoriq-image-rt-edge: demo image for Layerscape machines

For example:

```
$ bitbake imx-image-rt-edge
```

Build pre-built toolchains:
---------------
```
$ bitbake <image> -c populate_sdk
```
Be sure to replace image with an image (e.g. "qoriq-image-rt-edge")

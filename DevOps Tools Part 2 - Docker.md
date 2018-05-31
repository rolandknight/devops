# DevOps Tools Part 2 - Docker

## OS Preparation

Consider your entire VM to be Ephemeral - store all your data on external storage (or at least replicate local data) and you can swap out VMs (and hardware) with ease. Automate virtually all installation. Setup the bare minimum at the OS level and install everything else in containers.

### Bare Metal

1. Download Ubuntu Server ISO 16.04 from [here](http://releases.ubuntu.com/16.04/ubuntu-16.04.4-server-amd64.iso)
1. Create a bootable USB stick using [Rufus](https://rufus.akeo.ie/). Of course, you can always go old school and burn a CD-ROM with the ISO.
1. Install Ubuntu (be sure to select LVM - you'll thank me later!)

### Cloud

1. All cloud providers (Amazon AWS, Microsoft Azure, etc) have ready made virtual machine images of Ubuntu Server 16.04 ready to install.
   Go find one and spin up a virtual machine! You might also find [Vagrant](https://www.vagrantup.com/) or 
   [docker-machine](https://docs.docker.com/machine/get-started-cloud/) useful for this purpose.

### Windows 7/10

1. Use the same ISO from the Bare Metal install above and spin it up in [Virtual Box](https://www.virtualbox.org/). Alternatively use the one stop shop [Docker Toolkit](https://docs.docker.com/toolbox/toolbox_install_windows/) (removing virtualbox and rebooting is recommended before installation).

### Windows 10 Pro

Pro comes with hyperV so install the Ubuntu ISO and get going...

## Install Docker CE 18.03

### Linux (preferred)

1. Install using Rancher provided script:
    ```shell
    curl https://releases.rancher.com/install-docker/18.03.sh | sh
    ```
Note that on Redhat you may need a package repo to do this which requires a license (developer license will be fine for non-production).

### Windows 7

1. The simplest is to use [Docker Toolbox](https://github.com/docker/toolbox/releases/tag/v18.03.0-ce).
   This installs [VirtualBox](https://www.virtualbox.org/) and Docker all in one step.

### Other OS

1. Windows 10 or MAC OS, read through the [install page](https://docs.docker.com/install/) to install Docker CE 18.03.

## Fun With Docker - Primes <= 1000

Sure, you could use the simple docker hellow world image **hello-world** but that's super boring. Instead, lets find all prime numbers less than 1000!

We'll complete this task using a ready made [Node.js](https://nodejs.org/en/) docker image and some simple Javascript to generate primes less than 1000 (stolen and adapted from some random place in the internet).

Create a new directory and inside the file **primes.js** as follows:
```
var max=1000, D=[], primes=[];
for (var q=2; q<max; q++) {
	if (D[q]) {
		for (var i=0; i<D[q].length; i++) {
			var p = D[q][i];
			if (D[p+q]) D[p+q].push(p);
			else D[p+q]=[p];
		}
		delete D[q];
	} else {
		primes.push(q);
		if (q*q<max) D[q*q]=[q];
	}
}
console.log(primes.join())
```

### Run it!
```
docker run -v $PWD:/js -w /js node node primes.js
```
That's it! This will download the latest node.js, mount the current directory and execute inside node.js. Easy huh! Note that the second run will be almost instantaneous as the image is cached locally.

## What's Next?

Read up on [Docker](https://docs.docker.com/) and [LXC](https://linuxcontainers.org/lxc/introduction/) which is what Docker is based on. And get out there - start writing docker and docker-compose scripts.

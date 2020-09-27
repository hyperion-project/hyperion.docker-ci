# Docker Hyperion compilation
[![Docker CI](https://github.com/Hyperion-Project/hyperion.docker-ci/workflows/Docker%20CI/badge.svg)](https://github.com/orgs/hyperion-project/packages)<br>
Provides images (with various tags) to compile Hyperion inside a Docker container.<br>
Images are available at https://github.com/orgs/hyperion-project/packages

<br>

**Compile Hyperion ON x86_64 FOR os/hardware(arch) as cross compilation through qemu:**

|     Image:Tag     	|                os/hardware(arch)      	      	|
|:-----------------:	|:----------------------------------------------:	|
| `x86_64:stretch`  	| Debian Stretch/Generic (x86_64)                	|
| `x86_64:buster`   	| Debian Buster/Generic (x86_64)                 	|
| `x86_64:bullseye`   	| Debian Bullseye/Generic (x86_64)                 	|
| `armv6l:stretch`  	| Debian Stretch/Raspberry Pi v1 & ZERO (armv6l) 	|
| `armv6l:buster`   	| Debian Buster/Raspberry Pi v1 & ZERO (armv6l)  	|
| `armv6l:bullseye`   	| Debian Bullseye/Raspberry Pi v1 & ZERO (armv6l)  	|
| `armv7l:stretch`  	| Debian Stretch/Raspberry Pi 2, 3, 4 (armv7l)     	|
| `armv7l:buster`   	| Debian Buster/Raspberry Pi 2, 3, 4 (armv7l)      	|
| `armv7l:bullseye`   	| Debian Bullseye/Raspberry Pi 2, 3, 4 (armv7l)	|
| `aarch64:stretch` 	| Debian Stretch/Generic (AArch64)               	|
| `aarch64:buster`  	| Debian Buster/Generic (AArch64)                	|
| `aarch64:bullseye`  	| Debian Bullseye/Generic (AArch64)                	|

<br>

**Compile Hyperion ON Raspberry Pi(armv6l, armv7l) FOR os/hardware(arch) as native compilation:**

|        Image:Tag       	|                         os/hardware(arch)                        	|
|:----------------------:	|:----------------------------------------------------------------:	|
| `rpi-raspbian:stretch` 	| Raspbian Stretch/Raspberry Pi v1, ZERO, 2, 3, 4 (armv6l, armv7l) 	|
| `rpi-raspbian:buster` 	| Raspbian Buster/Raspberry Pi v1, ZERO, 2, 3, 4 (armv6l, armv7l)  	|

**Notes:** You can run these compiled binaries always on newer OS versions or comparable linux distributions, but never on older ones.

# Docker Hyperion compilation <br>[![Docker CI](https://github.com/Hyperion-Project/hyperion.docker-ci/workflows/Docker%20CI/badge.svg)](https://github.com/Hyperion-Project/hyperion.ng/packages)<br>Provides images to compile Hyperion inside a Docker container.
Images are available at https://github.com/Hyperion-Project/hyperion.ng/packages

Tag explanation: Compile Hyperion ON os/hardware(arch) FOR os/hardware(arch)
 - `x86_64` ON all(x86_64) FOR Debian Stretch (x86_64)
 - `armv6l` ON all(x86_64) FOR Debian Stretch (Raspberry Pi v1 & ZERO) as cross compilation through qemu
 - `armv7l` ON all(x86_64) FOR Debian Stretch (Raspberry Pi 2 & 3) as cross compilation through qemu
 - `aarch64` ON all(x86_64) FOR Debian Stretch (Generic AARCH64) as cross compilation through qemu
 - `rpi-raspbian-stretch` ON Raspberry Pi(armv6l, armv7l) FOR Raspbian Stretch (Raspberry Pi v1, ZERO, 2 & 3) as native compilation
 - `rpi-raspbian-buster` ON Raspberry Pi(armv6l, armv7l) FOR Raspbian Buster (Raspberry Pi v1, ZERO, 2 & 3) as native compilation

You can run these compiled binaries always on newer OS versions or comparable linux distributions, but never on older ones
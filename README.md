# Docker Hyperion compilation <br>[![Build Status](https://dev.azure.com/Hyperion-Project/Hyperion.NG/_apis/build/status/Hyperion.NG.Docker-CI?branchName=master)](https://dev.azure.com/Hyperion-Project/Hyperion.NG/_build/latest?definitionId=6&branchName=master)<br>
Provides images to compile Hyperion inside a Docker container.
Images are available at https://hub.docker.com/r/hyperionproject/hyperion-ci/

Tag explanation: Compile Hyperion ON os/hardware(arch) FOR os/hardware(arch)
 - `amd64` ON all(x86_64) FOR Debian Stretch (x64)
 - `amd64-buster` ON all(x86_64) FOR Debian Buster (x64)
 - `armv6hf` ON all(x86_64) FOR Debian Stretch (Raspberry Pi v1 & ZERO) as cross compilation through qemu
 - `armv6hf-buster` ON all(x86_64) FOR Debian Buster (Raspberry Pi v1 & ZERO) as cross compilation through qemu
 - `armv7hf` ON all(x86_64) FOR Debian Stretch (Raspberry Pi 2 & 3) as cross compilation through qemu
 - `armv7hf-buster` ON all(x86_64) FOR Debian Buster (Raspberry Pi 2 & 3) as cross compilation through qemu
 - `aarch64` ON all(x86_64) FOR Debian Stretch (Generic AARCH64) as cross compilation through qemu
 - `aarch64-buster` ON all(x86_64) FOR Debian Buster (Generic AARCH64) as cross compilation through qemu
 - `rpi-raspbian-stretch` ON Raspberry Pi(armv6hf, armv7hf) FOR Raspbian Stretch (Raspberry Pi v1, ZERO, 2 & 3) as native compilation
 - `rpi-raspbian-buster` ON Raspberry Pi(armv6hf, armv7hf) FOR Raspbian Buster (Raspberry Pi v1, ZERO, 2 & 3) as native compilation


You can run these compiled binaries always on newer OS versions or comparable linux distributions, but never on older ones

# Docker Hub Status
[![dockeri.co](https://dockeri.co/image/hyperionproject/hyperion-ci)](https://hub.docker.com/r/hyperionproject/hyperion-ci)

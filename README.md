# Docker Hyperion compilation
Provides images to compile Hyperion inside a Docker container.
Images are available at https://hub.docker.com/r/hyperionproject/hyperion-ci/

Tag explanation: Compile Hyperion ON os/hardware(arch) FOR os/hardware(arch)
 - `i386` ON all(x86_64) FOR Debian Stretch (x86)
 - `amd64` ON all(x86_64) FOR Debian Stretch (x64)
 - `armv6hf` ON all(x86_64) FOR Debian Stretch (Raspberry Pi v1 & ZERO) as cross compilation through qemu
 - `armv7hf` ON all(x86_64) FOR Debian Stretch (Raspberry Pi 2 & 3) as cross compilation through qemu
 - `aarch64` ON all(x86_64) FOR Debian Stretch (Generic AARCH64) as cross compilation through qemu

You can run these compiled binaries always on newer OS versions or comparable linux distributions, but never on older ones

# Docker Hub Status
[![dockeri.co](https://dockeri.co/image/hyperionproject/hyperion-ci)](https://hub.docker.com/r/hyperionproject/hyperion-ci)

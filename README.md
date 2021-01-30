# docker-oreboot

Build oreboot with Docker.

# Usage:

    git submodule init
    git submodule update

Then put file `bzImage` containing a Linux kernel (or LinuxBoot) into subdirectory `oreboot`, and then:

    ./prepare
    cd oreboot
    PAYLOAD_A="${PWD}/bzImage" ../build

Note that `build` needs a device tree compiler (`dtc`) on the host since it's not in the Docker image.

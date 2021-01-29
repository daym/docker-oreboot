# docker-oreboot

Build oreboot with Docker.

# Usage:

Put file `bzImage` containing a Linux kernel (or LinuxBoot) into subdirectory `oreboot`, and then:

    ./prepare
    cd oreboot
    PAYLOAD_A=${PWD}/bzImage ../build

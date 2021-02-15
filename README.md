# docker-oreboot

Build oreboot with Docker.

# Usage:

    git submodule init
    git submodule update
    egrep 'toolchain|channel|components' oreboot/rust-toolchain >oreboot/rust-toolchain.new && mv oreboot/rust-toolchain.new oreboot/rust-toolchain
    sed -i 's;"llvm-tools-preview", ;;' oreboot/rust-toolchain

Then put file `bzImage` containing a Linux kernel (or LinuxBoot) into subdirectory `oreboot`, and then:

    ./prepare
    cd oreboot
    PAYLOAD_A="${PWD}/bzImage" ../build

Note that `build` needs a device tree compiler (`dtc`) on the host since it's not in the Docker image.

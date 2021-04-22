# docker-oreboot

Build oreboot with Docker.

# Usage:

First, run

    ./prepare

This will prepare a Docker image with the Rust compiler from DockerHub and then it will check out `oreboot` into the subdirectory `oreboot`.

Then put file `bzImage` containing a Linux kernel (or LinuxBoot) into subdirectory `oreboot`, and then:

    cd oreboot
    cat rust-toolchain | egrep 'toolchain|channel|components' |sed -e 's;"llvm-tools-preview", ;;' >rust-toolchain.new && mv rust-toolchain.new rust-toolchain
    MAINBOARD=src/mainboard/amd/romecrb PAYLOAD_A="${PWD}/bzImage" ../build

Note that `build` needs a device tree compiler (`dtc`) on the host since it's not in the Docker image.

Other interesting invocations would be:

    RUSTFLAGS="--emit asm -C llvm-args=-x86-asm-syntax=intel" CARGO_NET_OFFLINE=true MAINBOARD=src/mainboard/amd/romecrb PAYLOAD_A=${PWD}/orebootkernel DTC=dtc ../build # This also leaves generated assembly code in the target dir (".s" files)

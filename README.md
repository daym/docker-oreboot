# docker-oreboot

Build oreboot with Docker.

# Usage:

    cat oreboot/rust-toolchain | egrep 'toolchain|channel|components' |sed -e 's;"llvm-tools-preview", ;;' >oreboot/rust-toolchain.new && mv oreboot/rust-toolchain.new oreboot/rust-toolchain

Then put file `bzImage` containing a Linux kernel (or LinuxBoot) into subdirectory `oreboot`, and then:

    ./prepare
    cd oreboot
    MAINBOARD=src/mainboard/amd/romecrb PAYLOAD_A="${PWD}/bzImage" ../build

Note that `build` needs a device tree compiler (`dtc`) on the host since it's not in the Docker image.

Other interesting invocations would be:

    RUSTFLAGS="--emit asm -C llvm-args=-x86-asm-syntax=intel" CARGO_NET_OFFLINE=true MAINBOARD=src/mainboard/amd/romecrb PAYLOAD_A=${PWD}/orebootkernel DTC=dtc ../build # This also leaves generated assembly code in the target dir (".s" files)

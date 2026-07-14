# Application SDK

### What is Application SDK?
> It's a set of tools and libraries that developers use to build their application for a specific target or operating systems.

### How to create an Application SDK?
> Simply run $ bitbake meta-toolchain
> ps. meta-toolchain is a recipe responsiple for cooking the SDK where can found under meta layer

### What variables shall be configured/defined to store the libs needed for compilation?
> TOOLCHAIN_HOST_TASK = " qtbase ": holds host tools the might be required for application dev. on host as sysroot
> TOOLCHAIN_TARGET_TASK: holds target tools the might be required for application dev. on target as sysroot

### what is tmp/deploy/sdk/<..>/toolchain.sh?
> It's a script being generated after cooking the toolchain recipe. after running it, a sysroot will be created for both host and target which holds all the required libs.

### How to use the SDK?
> Source the script **environment-setup-target-poky-linux**

### How to exted the SDK application?
> Simply create a recipe in your application layer and extend the above variables with the required configuration.  
> Example: append the TOOLCHAIN_TASKs with qt toolchain.
```bash
TOOLCHAIN_HOST_TASK:append = " \
    packagegroup-qt6-essentials \
"

TOOLCHAIN_TARGET_TASK:append = " \
    nativesdk-packagegroup-qt6-toolchain-host-essentials \
"
```

### How to adapt your Makfile and CMake to point to your sysroot?
> consider using this environment variable "OECORE_TARGET_SYSROOT"
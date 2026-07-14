### List some tasks that might be executed for recipes?
```
do_fetch
Purpose: Fetches the source code and other resources (like patches or tarballs) from remote or local sources. This is typically the first task in the build process.
Why important: You need to understand how the source is fetched, especially when dealing with external repositories or files.

do_unpack
Purpose: Unpacks the fetched source into a working directory.
Why important: Unpacking the sources correctly is essential to ensure they are in the right format for the subsequent build tasks.

do_patch
Purpose: Applies patches to the source code.
Why important: Often used to apply custom fixes or modifications to the source code before building it.

do_configure
Purpose: Configures the build system for the software (e.g., running ./configure or cmake).
Why important: This is necessary for setting up the environment and flags before the actual compilation.

do_compile
Purpose: Compiles the source code into binaries or libraries.
Why important: This is the core task where the software is built, and you need to understand how to handle this step for different build systems.

do_install
Purpose: Installs the compiled binaries and other files into the target system.
Why important: You need to understand how to correctly install the built software into the sysroot or final image.

do_package
Purpose: Packages the installed files into a specific package format (like .deb, .rpm, or Yocto's .ipk).
Why important: This task is necessary when creating installable packages from the built software.

do_image
Purpose: Creates the final image (e.g., .iso, .img) for deployment.
Why important: Essential for generating a bootable or deployable image, which is the final product after all the software and configurations are in place.

do_clean
Purpose: Cleans up the working directory and removes previously built files.
Why important: This is useful for starting fresh when the build process has errors or needs to be re-run from scratch.

do_test
Purpose: Runs tests on the built software, typically unit tests or integration tests.
Why important: Ensuring the quality and correctness of the built software before packaging and deployment.
```

### SCR_URI variable?
> it's a variable hold file paths, which these files can be found by so running tasks.


### what is the default patching tool of Yocto?
> Quilt

### Remark:
> beaglebone-poky-linux-gnueabi: Output specific to BeagleBone Black (final target image).    
> The key directory you should focus on is the beaglebone-poky-linux-gnueabi directory since it contains the build artifacts for your BeagleBone Black. You can look there for the root filesystem, kernel, and any target-specific binaries that will be flashed to your BeagleBone Black.

### Cook a simple Hello-World-Recipe?
> Mostly, these are the tasks steps
> [Fetch - Decompress - Patch - Configure - Compile - Install - Deploy&Package(rpm, ipk, deb) - Q/A]  
``` bash
# Note: Variables are all CAPS
SUMMARY = "Hello World C file recipe"

# This variable is optional
SECTION = "base"

LICENCE = "CLOSED"

## Package information are extracted from the file name, so they can be ignored
## FileName-Format: helloworld_1.0.0.bb  OR helloworld_git.bb
## The 3 below variables would be filled out automatically, after naming the file the prior format.
# Project Revision
PR = "r0"
# Package Name
PN = "helloWorldC"
# Package Version
PV = "Package Version"

## A variable that holds any file shall be existed in the directory, it shall hold paths of any kind of files or directories i.e. gitRepos, tarball, local files .. etc.
SCR_URI = "file://helloyp.c \ "

# ${WORKDIR}: recipe root location
# ${D}: comilation outputs should go to a specific destination under image, as do_image task run can see copilation output files.
# ${S}: source directory 
# ${B}: build directory (by defualt it's the same path as {S} but can be overwritten)
# ${bindir}: it's usr/bin/
# ${CC}: a variable referes to gcc compiler


############################# A cookable hello recipe task #############################
SUMMARY = "Hello World C file recipe"
LICENSE = "CLOSED"

SRC_URI = " file://src "

S = "${WORKDIR}/src"

TARGET_CC_ARCH += "${LDFLAGS}"

do_compile () {
    ${CC} ${S}/helloyp.c -o ${B}/helloyp
}

do_install () {
    install -d ${D}${bindir}
    install -m 755 ${S}/helloyp ${D}${bindir}/
}
```

## Recipes for GNU Make
### Write a recipe for a GNU Make application?
```bash

```

### Whe to use RDEPENDS / runtime dependancy variable?
> Use RDEPENDS in Yocto recipes when you need to ensure that other packages or services are available for the application to function properly at runtime. This makes dependency management more efficient and reduces the risk of runtime failures due to missing dependencies.

### Whe to use DEPENDS / compile dependancy variable?
> tools, libraries needed to build the software

### How to run a make command inside the do_compile task?
> $ oe_runmake    ==> To ensure a well proper env to run make

### When can you ignore writing do_install taks?
> if it's already implemented in the Makefile.

### what is the variable STAGING_DIR_TARGET?
> It points to the package recipe-sysroot directory for target build, i.e. arm machine.

### what is the variable STAGING_DIR_HOST?
> It points to the package recipe-sysroot directory for host/native build, i.e. x86 machine.

## Basic Recipes for CMake
### Is CMake supported by Yocto?
> yes, its class definition located in the meta layer cmake.bbclass  
> it provides many tasks, so no need to define all taks yourself, only you should inherit them.

### How to inherit a class?
> run $ inherit <ClassName>
> example: $ inherit cmake


#### How to make Makefile compatible with yocto?


##### List most known recipes variables?
> ${STAGING_DIR_TARGET} : points to recipe sysroot-target (arm) directory **/poky/build/tmp/work/beaglebone-poky-linux-gnueabi/<PackageName>/<PackageRevision>/recipe-sysroot/**
> ${STAGING_DIR_HOST} : points to recipe sysroot-native directory (x86)
> ${S}
> ${B}
> ${D}
> ${bindir} : ${STAGING_DIR_TARGET}/usr/bin
-----

##### How to pass a vairable to  the cmake or makefile?
> EXTRA_OECMAKE
> EXTRA_OEMAKE
### DEGBUG
###### How to check the tasks that run for a specific recipe?
> cat the file **/poky/build/tmp/work/beaglebone-poky-linux-gnueabi/<PackageName>/<PackageRevision>/temp/log.taks_order** .. check this file **log.taks_order**

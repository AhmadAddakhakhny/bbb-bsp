# Machine Configuration

### What are MACHINE file?
>  a one or more files that have various variables that defined inside.
> they can be found under the path /<layer-name>/conf/machine/*.conf
> In order to make the machine active in the oe build, you have to set the machine variable inside the poky/build/local.conf 

### List the important variables to be considered to write a prober machine config file?
> TARGET_ARCH = "arm" : Setting processor architecture.  
> PREFERRED_PROVIDER_virtual/kernel = "linux-bb.org" : Mention that linux beaglebone kernel to be used.  
> MACHINE_FEATURES += "screen" : Setting predetermind features to be (de)activated after booting the target. there might be some packaheg depend on a specific feature to be built. and also might execlude a specific feature as I don't want my target to support keyboard.
> SERIAL_CONSOLES = "115200;ttyS0" : Setting serial consol boudrate and device file name.  
> KERNEL_IMAGETYPE = "zImage" : type of the generated image wether uImage or zImage  
> IMAGE_FSTYPES += "tar.xz wic.xz wic.bmap" : setting compression format of the image  

### List the categories of the MACHINE variables?
> Variables to tune the compile, and support for processor optimization. i.e. TARGET_ARCH  
> Variables to define default applications. i.e. PREFERRED_PROVIDER_virtual  
> Variables to define kernel image, boot command for the kernel device tree. i.e. KERNEL_BOOTCMD  
> Variables to define U-Boot properities. i.e. UBOOT_SUFFIX, UBOOT_MACHINE  
> Variables to defince serial consoles to be used by kernel. i.e. SERIAL_CONSOLES

### Remark
> There are much more variables to be learned but the above are essential to know  
> There are some files might be included to the machine config files as the contain some of the mentioned variables with initial definition.

### Question?
> What is the difference between MACHINE_FEATURES, DISTRO_FEATURES and IMAGE_FEATURES? 
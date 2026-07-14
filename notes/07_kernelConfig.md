# Kernel Configuration

### How to enable or disable a kernel module?
> By setting or clearing some MACROS.
> if MACRO=y means the module will be integrated in the kernel, else if MACRO=m the module would be loadable during the user space.
> Kernel module customization used to reduce the boot time as much as possible.

### What to do be fore enabling a specific kernel module?
> Check its dependancies and enable them as well.

### How to customize the kernel Module via Yocto Project?
> you have to extend the kernel-recipe of your BSP.  
> in your application layer, extented recipes-kernel of your SoC. i.e. linux-bb.org_git.bb  
> and write out CONFIG_SPI=y

### How to check a Kernel Moudle dependancy?
>  check the SRC_URI of the kernel-recipe of your target to direct you to the github repo and base-kernel-configuration-file-name
https://github.com/beagleboard/linux/blob/v6.1.80-ti-r34/arch/arm/configs/bb.org_defconfig  
> or $ cat /poky/build/tmp/work-shared/beaglebone/kernel-source/arch/arm/configs/bb.org_defconfig  

### How to check if your changes got considered?
> go to your image build to .config file. $ cat poky/build/tmp/work/<target>/linux-standard-build/.config
> if you flashed the image, cat this file. $ cat /proc/config.gz | gunzip


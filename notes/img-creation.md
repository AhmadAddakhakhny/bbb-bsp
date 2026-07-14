### How to create SD-Card image?
> ###### a. List deploying steps of the BBB using wic tool?
> 1. go to: /BBB-YoctoProject/poky/build/deploy-ti/images/beaglebone/
> 2. de-compress core-image-minimal-beaglebone.wic.xz
> $ unxz core-image-minimal-beaglebone.wic.xz
> 3. partition your SD-Card and flash it with the .wic file
> sudo dd if=core-image-minimal-beaglebone.wic of=/dev/sdc status=progress bs=4096 && sync
> 4. plug the DS-Card to your BBB and boot it up
> 5. username = root, password = "NO PASSWARD REQUIRED"
> Remark: wic is a tool provided by Yocto Project, dd is a core utility tool
> note: use g-parted or balena etcher


##### How to create eMMC image?
# RAUC OTA and dm-verity concepts

### Types of updates
1. packages (rfs composes packages)
```bash
## a package manager required, and the follwoing managers are supported by Yocto
# 1. dnf/rpm-> .rpm   # 2. apt/dpkg-> .deb   # 3. opkg-> .ipk
```
> 1.1. on the target, there would be a client application that runs the sudo apt install, after approving such operation from the server side (user server)  
> 1.2.problems of this step is there is no roll-back to a pervious version, so we should have 2 rootfs  
> 1.3. now, the system running on rootfs-a so the package would be installed on rootfs-b then switch to rootfs-b. then reboot.  
> 1.4. during bootup, the kernel would mount the rootfs-b, and the kernel shall know such info by 'kernel arguments' (**root=id**)that shall be passed by the u-boot.  
> Remark: U-boot would load the dtb and kernel in the RAM, and the arguments shall be inserted in the dtb and then jumbs to the kernel.  
> to update the (**root=id**) argument where located in the boot partition, it depends on the BL, so check how shall be done for u-boot.  
```bash
# uboot.env
boot_device = a/b
bootargs root=${boot_device}
```
> step to perform the update:
```bash
mount rootfs-b
dpkg -i xyz
# How to update uboot.env from userspace?
fw_setenv boot_device b # set the uboot.env argument with the intended partion to be used after bootup
reboot
```

2. partition  
3. kernel + DTB  
> in this case, there would be 2 kernel versions  
> partitioning would be [BootA, BootB, u-boot.env, rootfsA, rootfsB]
4. BL (uboot) + DTB
5. OPTEE (ARM)
6. TF-A (ARM)

### List SW update tools:
1. RAUC (meta-rauc)
2. Mendor
3. SWupdate

### RAUC
> rauc applies this concept slot - which using a file system type squashfs (rootfs-type) which smaller than ext4 and its feature is (read-only)
> slot = boot; (file-bootParition) | slot = rootfs; (image)
> rauc generates a bundle (file.raucb)
> RAUC works in bloks if **adaptive** feature enabled, which means if the bundle is 200MByte it would be segmented into 4KBytes and the rauc checks the each 4KBytes and when differences happens updates this section.  

### Yocto + RAUC
1. Bitbake output:  
1.1. boot (FAT) + rootfs (ext4)  
1.2. Bundle (squashfs)  
2. rauc install https://xyz/bundle.raucb
3. install the boot-b & rootfs-b and update u-boot.env with partion-b
4. reboot

### dm-verity or (dm-crypt)
> it's a security feature  
> secure boot: used to veritfy the integrity of the **rootfs** during boot by means of hash-tree.  
> advaced feature: to mangage the hash-tree and dor hash-format, to know how to manage the segementation of the bundle of 200Mbytes to 4KBytes in pieces.
---
## Implementation tips
0. Check the rauc documentations 
1. Yocto level: Add meta-rauc layer [meta-rauc-community]
2. configure rauc recipes to define the required partitions [boot-1, boot-2, uboot-env, rootfs-1, rootfs-2], AKA slot definition + data hooks 
3. segment your emmc or sdCard
4. system.conf file to configure rauc application /etc/rauc/
5. link the rauc to BL (u-boot) using the system.conf
6. note: rauc doesn't support automatic scripts for rolling over partions for uboot, do it manually. 
> it provides 
```bash
BOOT_ORDER="A B"
BOOT_A_LEFT=3
BOOT_B_LEFT=3 # 3 Means number of trials
MODE=valid    # MODE shall be either valid or trial
```
7. After the reboot ... if a kernel panic took place (which means this bundle is corrupted), the HW watchdog would reset the SoC and keep booting the same kernel image with the assigned trial numbers in BOOT_A/B_LEFT variable. in case number of trials over, the MODE variable would be retrieved to valid (not any more in trial mode) and the BOOT_ORDER would be changed back. 
8. Remark: what are the requirements that defines that your userspace application is statisfied and no need for roll back? 
9. then mark the bundle as good (valid) (run as script to manage after-boot in userspace at success and failure)
---

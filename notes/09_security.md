# Secure Boot

#### What is boot process?
> it's flow depends on processor of the chip, and mostly the sarting of it changes.
> App <- init-Manager(systemd)(sysvinit)(busybox) <- kernel <- bootloader (uboot)(barebox) <- FSB
> ROM BL -> SPL -> U-Boot -> Kernel
> Arm now supports (trusted zone)
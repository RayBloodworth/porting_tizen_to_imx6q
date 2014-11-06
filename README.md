porting_tizen_to_imx6q
======================
Build u-boot, kernel and gpu libs with yocto:

MACHINE=imx6qsabresd source fsl-setup-release.sh –b build-wayland -e wayland
bitbake core-image-weston

Note:These images should be built with sfp.To enable sfp,add the followed line into your local.conf:
DEFAULTTUNE_mx6 = "cortexa9-neon"

Note:You should use 3.10.31 or above to support weston1.5

==================================================
Flash u-boot, device tree and kernel to SD card:

sudo dd if=u-boot-imx6qsabresd_sd.imx of=/dev/sdb bs=512 seek=2 conv=fsync

sudo dd if=uImage_imx_v7_defconfig of=/dev/sdb bs=512 seek=2048 conv=fsync
This will copy uImage to the media at offset 1 MB (bs x seek = 512 x 2048 = 1MB).

sudo dd if=uImage-imx6q-sabresd.dtb of=/dev/sdb bs=512 seek=20480 conv=fsync
This will copy uImage-imx6q-sabresd.dtb to the media at offset 10 MB (bs x seek = 512 x 20480 = 10MB).
===================================================
boot from SD card:

setenv loadaddr 0x12000000
setenv fdt_addr 0x18000000
setenv fdt_high 0xffffffff

setenv bootargs console=ttymxc0,115200 root=/dev/mmcblk2p1 rootwait rw printk.time=1 rootfstype=ext4 init=/sbin/init systemd.log_level=debug systemd.log_target=console systemd.sysv_console=1 video=mxcfb0 :dev=ldb,LDB-XGA,if=RGB666 video=mxcfb1:dev=hdmi,1920x1080M@60,if=RGB24

=============================================================
Create a Tizen IVI image with the ks file from:
https://download.tizen.org/releases/daily/tizen/ivi-3.0.m14.3/tizen-3.0.m14.3-ivi_20141022.3/images/arm/ivi-mbr-renesas-m2/tizen-3.0.m14.3-ivi_20141022.3_ivi-mbr-renesas-m2.ks

Or modify the atom ks file:
tizen-3.0.m14.3-ivi_20141022.3_ivi-mbr-i586.ks

sudo mic create loop your_ks_file

copy to SD Card partition:
 
sudo dd if=platform.img of=<sd card partition>

==============================================================

Booting the platform
Connect a micro USB cable to Serial 0 on the board , ACC ON and then from use a terminal program to connect at 115200 baud

==============================================================


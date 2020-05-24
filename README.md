# Assignment 4


## Step 1

Before everything else, we need to install or update all needed packages using following commands (Ubuntu 64 bits 14.04 and +):
```bash
sudo apt-get update && sudo apt-get upgrade
sudo apt-get install git-core gnupg flex bison gperf build-essential zip curl zlib1g-dev gcc-multilib g++-multilib libc6-dev-i386 lib32ncurses5-dev x11proto-core-dev libx11-dev lib32z-dev libgl1-mesa-dev libxml2-utils xsltproc unzip

```

## Step 2

Once the system up-to-date, we have to sync Android Source (AOSP)
```bash
repo init -u https://github.com/tab-pi/platform_manifest -b nougat
repo sync
```

## Step 3

The step 2 can be very long, then we need to build Kernel
Then go into the "kernel/rpi folder".
```bash
sudo apt install gcc-arm-linux-gnueabihf
cd kernel/rpi
```
And build our kernel with those commands (you have to use bash).
```bash
ARCH=arm scripts/kconfig/merge_config.sh arch/arm/configs/bcm2709_defconfig android/configs/android-base.cfg android/configs/android-recommended.cfg
ARCH=arm CROSS_COMPILE=arm-linux-gnueabihf- make zImage
ARCH=arm CROSS_COMPILE=arm-linux-gnueabihf- make dtbs
```

## Step 4

Now we need to install the python mako module

```bash
sudo apt-get install python-mako
```

## Step 5

Now we can build our android source with following commands.
```bash
cd ../..
source build/envsetup.sh
lunch rpi3-eng
make ramdisk systemimage
```
This step can also take a lot of time.

## Running steps

You have to prepare your SD card following this set-up:

- p1 512MB for BOOT
- p2 1024MB for /system
- p3 512MB for /cache
- p4 remainings for /data

Then you can copy custom android OS on the sd-card into the corresponding partition (from their output folder). 
- device/brcm/rpi3/boot/* to p1:/
- kernel/rpi/arch/arm/boot/zImage to p1:/
- kernel/rpi/arch/arm/boot/dts/bcm2710-rpi-3-b.dtb to p1:/
- kernel/rpi/arch/arm/boot/dts/overlays/vc4-kms-v3d.dtbo to p1:/overlays/vc4-kms-v3d.dtbo
- out/target/product/rpi3/ramdisk.img to p1:/

## HDMI

If DVI monitor does not work try following for p1:/config.txt

```bash
hdmi_group=2
hdmi_mode=85
```

## Running screens

We don't have any raspberry so we can't have any running screens of the application

# HIFIVE-PREMIER-P550-Ubuntu
Ubuntu Releases for HIFIVE PREMIER P550 Board.

## Description

Ubuntu Image releases for HIFIVE PREMIER P550 Board.
- Based on Ubuntu 24.04.2 LTS.
- Prebuilt Ubuntu image in compressed format named `dvb-ubuntu-24.04-preinstalled-server-riscv64.img.zst`.
- Please ensure that the validated combination of the bootloader image and the Ubuntu image are flashed to the board. The release notes provide the version and validation details.
- The latest images release is available [here](https://github.com/eswincomputing/hifive-premier-p550-ubuntu/releases/tag/2025.07.30).

## Hardware preparation
- One Type-C USB cable (for serial port monitor and uboot command)
- One USB flash driver that at least 16G (image source)

## Flashing images
Copy the bootloader and uncompressed Ubuntu image to an **ext4** formatted the USB flash driver

For example
```
USB    /- bootloader_P550.bin
        |- dvb-ubuntu-24.04-preinstalled-server-riscv64.img
```
connect the USB flash driver to the **bottom** usb port of the  board and power up and wait for the uboot shell through the serial port.

### Bootloader flashing

If your device already has a compatible bootloader installed, you can skip this step.
```
=> usb reset
=> ls usb 0
=> ext4load usb 0 0x90000000 bootloader_P550.bin
=> es_burn write 0x90000000 flash
```

Demo output
```

=> usb reset
resetting USB...
Bus usb1@50490000: Register 2000140 NbrPorts 2
Starting the controller
USB XHCI 1.10
scanning bus usb1@50490000 for devices... 4 USB Device(s) found
       scanning usb for storage devices... 1 Storage Device(s) found
=>
=> ls usb 0
<DIR>       4096 .
<DIR>       4096 ..
      7714209280 dvb-ubuntu-24.04-preinstalled-server-riscv64.img
         4496952 bootloader_P550.bin
=> ext4load usb 0 0x90000000 bootloader_P550.bin
4496952 bytes read in 27 ms (158.8 MiB/s)
=>  es_burn write 0x90000000 flash
SF: 224 bytes @ 0x0 Read: OK
FIRMWARE writing...
Bootspi flash write protection disabled
Erase progress: 100%:++++++++++++++++++++++++++++++++++++++++++++++++++
SF: 2004 bytes @ 0x140000 Erased: OK
Write progress: 100%:++++++++++++++++++++++++++++++++++++++++++++++++++
SF: 0x7d4 bytes @ 0x140000 Written: OK
Bootspi flash write protection enabled
DDR writing...
Bootspi flash write protection disabled
Erase progress: 100%:++++++++++++++++++++++++++++++++++++++++++++++++++
SF: 455808 bytes @ 0x40000 Erased: OK
Write progress: 100%:++++++++++++++++++++++++++++++++++++++++++++++++++
SF: 0x6f480 bytes @ 0x40000 Written: OK
Bootspi flash write protection enabled
BOOTLOADER writing...
Bootspi flash write protection disabled
Erase progress: 100%:++++++++++++++++++++++++++++++++++++++++++++++++++
SF: 4038328 bytes @ 0x1c0000 Erased: OK
Write progress: 100%:++++++++++++++++++++++++++++++++++++++++++++++++++
SF: 0x3d9eb8 bytes @ 0x1c0000 Written: OK
Bootspi flash write protection enabled
Bootspi flash write protection disabled
Erase progress: 100%:++++++++++++++++++++++++++++++++++++++++++++++++++
SF: 224 bytes @ 0x0 Erased: OK
Write progress: 100%:++++++++++++++++++++++++++++++++++++++++++++++++++
SF: 0xe0 bytes @ 0x0 Written: OK
Bootspi flash write protection enabled
bootloader write OK
=>
```

:::warning
If you are using a bootloader version prior to `2025.06.30` and want to  upgrade to bootloader version `2025.06.30` or later, you cannot directly use the `es_burn` command to flash the bootloader; instead, you must flash it following the `RECOVER BOOTLOADER` method.
:::

### Ubuntu image burning
```
=> es_fs write usb 0 dvb-ubuntu-24.04-preinstalled-server-riscv64.img mmc 0
=> reset
```
Demo output
```
=> es_fs write usb 0 dvb-ubuntu-24.04-preinstalled-server-riscv64.img mmc 0
Write progress:  87%:+++++++++++++++++++++++++++++++++++++++++++
```

### Essdk deb packeges install

If you find that installing essdk deb packages using `apt install` is too slow, you can download the essdk and ffmpeg deb packages from essdk_ffmpeg_0630.zip [here](https://github.com/eswincomputing/hifive-premier-p550-ubuntu/releases/tag/2025.07.30) and install them by `dpkg -i XXXX.deb`.

## Download from network disk

If you are unable to download images from GitHub and you are in China, you can try downloading them from [here](https://pan.baidu.com/s/1gUJC8K_bSrhRO6P63BYV_w?pwd=p41j).

## Login to the board Using Serial Console

Default username and password for ubuntu image is `ubuntu`.
On first boot, user will be prompted to change the default password.

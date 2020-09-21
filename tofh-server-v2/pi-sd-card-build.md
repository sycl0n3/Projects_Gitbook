# Pi SD Card Build

## Download Raspberry Pi OS 32 Bit Lite

{% embed url="https://www.raspberrypi.org/downloads/raspberry-pi-os/" %}

## Using Etcher, install OS from Zip file into SD Card

## Enable ssh

Create a file called ssh on the boot partition of the SD card

## Enable Ethernet Over USB \(USB Gadget mode\)

### Edit config.txt <a id="step-4-edit-configtxt"></a>

* In the root folder of the SD card, open **config.txt** \(`/Volumes/boot/config.txt`\) in a text editor
* Append this line to the bottom of it:

  ```text
    dtoverlay=dwc2
  ```

* Save the file

### Edit cmdline.txt <a id="step-5-edit-cmdlinetxt"></a>

* In the root folder of the SD card, open **cmdline.txt** \(`/Volumes/boot/cmdline.txt`\) in a text editor
* After **rootwait**, append this text _leaving only one space_ between **rootwait** and the new text \(otherwise it might not be parsed correctly\):

  ```text
    modules-load=dwc2,g_ether
  ```

* If there was any text after the new text make sure that there is _only one space between that text_and the new text
* Save the file


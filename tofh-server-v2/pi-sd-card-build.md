# Pi SD Card Build

## Download Raspberry Pi OS 32 Bit Lite

{% embed url="https://www.raspberrypi.org/downloads/raspberry-pi-os/" %}

## Using Etcher, install OS from Zip file into SD Card

## Enable ssh

Create a file called ssh on the boot partition of the SD card

```text
touch /Volumes/boot/ssh
```

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

On a fresh image that has never been booted, you may see extra text after **rootwait**. But if you boot the pi from the disk at least once, that extra text may go away. That is why you must put the new text directly after **rootwait** - so it doesn’t get accidentally deleted.

### Boot the Pi Zero <a id="step-6-boot-the-pi-zero"></a>

* Put the SD card into the Pi Zero
* Plug a Micro-USB cable into the **data/peripherals** port \(the one closest to the center of the board – see picture above\)
* You do NOT need to plug in external power – it will get it from your computer
* Plug the other end into a USB port on your computer
* Give the Pi Zero plenty of time to bootup \(can take as much as 90 seconds – or more\)
* You can monitor the **RNDIS/Ethernet Gadget** status in the **System Preferences / Network** panel \(note that the IP address listed is not the host\)

### Login over USB <a id="step-7-login-over-usb"></a>

This part assumes that **ssh** is enabled for your image and that the default user is **pi** with a password of **raspberry**.

* Open up a terminal window
* Run the following commands:

  ```text
    ssh-keygen -R raspberrypi.local
    ssh pi@raspberrypi.local
  ```

* If the pi won’t respond, press Ctrl-C and try the last command again
* If prompted with a warning just hit enter to accept the default \(**Yes**\)
* Type in the password – by default this is **raspberry**

## Change the password

```text
passwd
```

## Update OS software

```text
sudo apt-get update
sudo apt-get upgrade

```

## Enable ssh and expand OS partition to full size of SD card

```text
sudo rasp-config
```

Enable and resize OS partition size.

## Preparing to install Unifi Controller

Ref: [https://pimylifeup.com/rasberry-pi-unifi/](https://pimylifeup.com/rasberry-pi-unifi/)

### Install OpenJAVA 8

```text
sudo apt install openjdk-8-jre-headless
```

### Install rng-tools to make the Unifi Controller quicker

```text
sudo apt install rng-tools
```


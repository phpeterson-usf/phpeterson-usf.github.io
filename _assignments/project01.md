---
layout: assignment
title: Project01 - Raspberry Pi Setup
due_date: "2022-02-02 14:00:00 -0800"
---

## Requirements

In lab, instructor/TA will check:
1. ssh onto your pi
1. clone a git repo from github.com (must show Internet access)
1. Use gcc and an editor of your choice to create, compile, and run Hello World in C.

## Install Raspberry Pi OS (Linux)

1. Go to [https://www.raspberrypi.org/downloads/](https://www.raspberrypi.org/downloads/)
1. Download the **Imager** for your laptop's OS, and install it
1. The imager allows you to "burn" or "flash" the OS image onto the Micro SD card
1. Under Operating System, we recommend selecting **Raspberry Pi OS (other)** and then **Raspberry Pi OS Lite (32 bit)**. 
1. If you want to install one of the desktop GUI builds and plug your Pi into a keyboard, mouse, and monitor, that's fine. Lite is a smaller download and adequate for our projects.
1. Insert your Micro SD card or adapter into your laptop or hub. Your laptop may mount the SD card. 
1. Under **Storage**, select your card

## Option 1: Connect your Pi to WiFi Directly

1. Type **Ctrl-Shift-X** to bring up the **Advanced Options** Dialog
1. Click the checkbox for **Set Hostname**, and give your Pi a unique hostname, perhaps your USF username
1. Click the checkbox for Enable SSH
1. Click the radio button for **Use Password authentication** and use a good password
1. Click the **Configure wifi** checkbox and use your WiFi network's SSID and password
1. Click the **Wifi country** menu and choose **US**
1. Click the **Set locale settings** checkbox, and set the time zone and keyboard layout, typically **America/Los Angeles** and **US**
1. Click the checkbox for **Skip first-run wizard**
1. Click **Save** in **Advanced Options**, and click the **Write** button in the Imager window. Writing and verifying the OS image will take a few minutes.

The Imager leaves the SD card unmounted, so you can remove the SD Reader from your computer

If you get errors from your computer that the SD card can't be read, just click Cancel (saw this on Windows 10)

## Option 1 if you live on campus

1. USF requires special steps to connect a Raspberry Pi to the campus network.
1. You can use [mydevices.usfca.edu](https://mydevices.usfca.edu) to tell USF to permit your device's MAC (hardware network) address on the network.
1. You can obtain the MAC address by using the `ifconfig` command on your Pi
    ```
    $ ifconfig
    eth0: flags=4099<UP,BROADCAST,MULTICAST> mtu 1500
    ether b8:27:eb:1c:9b:22
    ```
    The string of characters after `ether` is the MAC address. 
1. Purchase an Ethernet switch ([example 1](https://www.amazon.com/NETGEAR-Gigabit-Ethernet-Unmanaged-1000Mbps/dp/B00KFD0SMC/), [example 2](https://www.amazon.com/NETGEAR-5-Port-Gigabit-Ethernet-Unmanaged/dp/B07S98YLHM/), [example 3](https://www.amazon.com/Ethernet-Splitter-Optimization-Unmanaged-TL-SG105/dp/B00A128S24/)) and three Ethernet cables. 
1. Connect the WAN port of the switch into the Ethernet jack in your dorm room.
1. Connect the Pi's Ethernet port into one of the LAN ports on the Ethernet switch
1. Connect your laptop to one of the LAN ports on the Ethernet switch. This may require a USB-to-Ethernet adapter.
1. Now you should be able to `$ ssh pi@yourpi.local` and once on your Pi `ssh git@github.com`

## Option 2: Sharing Your Computer's Wi-Fi

1. The advantage of using your computer's Wi-Fi is that you don't have to enter network names and passwords on your Pi. USF Wireless is especially inconvenient for Raspberry Pi WiFi. Instead, you can share your laptop's Wi-Fi connection to your Pi, using the Pi as an **Ethernet Gadget** using the RNDIS network protocol. 
1. Open your Terminal app and  
`$ cd /Volumes/boot`
1. Edit `config.txt` and add this line to the bottom of the file, ensuring that there is a newline afterwards:
`dtoverlay=dwc2`
1. Edit `cmdline.txt` and insert this into the line, right after `rootwait`, ensuring that there is a single space on each side of what you add:
`modules-load=dwc2,g_ether`
1. `$ touch ssh` so that Raspberry Pi OS will start the SSH server when it boots
1. `$ cd ~` so you can **Eject** the SD card, remove it, and insert the Micro SD card into your Pi
1. On your laptop, enable Internet sharing:
    1. On macOS: Go to **System Preferences | Sharing**. Check **Internet Sharing**. Share your connection from Wi-Fi to computers using **RNDIS/Ethernet Gadget**
    1. On Windows: TBA
    1. On Linux: TBA
1. Plug your Pi into your laptop to power it up. After about 30 seconds (first boot takes longer), you can ssh onto your Pi:  
`$ ssh pi@raspberrypi.local`
1. The default password is **raspberry**. You should change that immediately using the command:  
`$ passwd`
1. RNDIS will make up new MAC addresses every time we connect, but we want to have constant MAC addresses, so we will set them into the `rndis.conf` file like this:
    ```
    $ dmesg | grep usb0 should produce something like:
    [ 6.315112] usb0: HOST MAC 07:17:57:07:1e:ab 
    [ 6.318300] usb0: MAC 5a:50:5e:5e:5f:5b
    ```
1. Put those addresses into rndis.conf like this:
    ```$ sudo bash
    $ cd /etc/modprobe.d
    $ cat > rndis.conf
    options g_ether host_addr=07:17:57:07:1e:ab dev_addr=5a:50:5e:5e:5f:5b
    ^D  (that's CTRL-D)
    ```
1. `$ sudo reboot` for the MAC address changes to take effect

## Install Software
1. At this point, your Pi should be able to connect to Internet sites, either using Internet Sharing, or directly on Wi-Fi. Check this using the command:  
`$ ping usfca.edu `  
and see that you get ICMP timings back. CTRL-C to stop ping
1. Raspberry Pi OS Lite may not come with a `git` client. You can check using the command:  
`$ which git`  
If the `git` client is not found, you can install it using the command:  
`$ sudo apt-get install git`
1. Raspberry Pi OS comes with the text editors `vi` and `vim`. If you prefer to use emacs or [micro](https://micro-editor.github.io/) you may install those. 
1. Cyberduck and similar are not recommended. You will lose credit in Interactive Grading for using Sublime or VSCode.
1. Install git and pip:
    ```
    $ sudo apt-get install python3-pip
    $ sudo apt-get install git
    ```
## Powering your Pi Safely
1. Since Raspberry Pis have no battery, disconnecting power while the OS is running can lose your changes, or even corrupt the filesystem, rendering the OS unbootable.
1. Always use  
`$ sudo shutdown now`  
to power down the board when you're not using it.
1. If you're powering a larger Pi (model 3b+ or 4b) you should a separate AC adapter, rather than powering it from the USB port on your computer.
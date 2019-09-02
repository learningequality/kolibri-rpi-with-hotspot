Setting up Kolibri on a Raspberry Pi with Hotspot
=================================================

This sets up the latest version of Kolibri on a Raspberry Pi (currently, designed for one with built-in wifi -- e.g. Pi 3B+, Pi Zero W, or Pi 4), along with an open hotspot called "kolibri". Anybody who connects to that hotspot will be directed in their browser to the instance of Kolibri running on the Raspberry Pi.

1. Download the latest version of Raspbian Lite from https://www.raspberrypi.org/downloads/raspbian/
1. Use https://www.balena.io/etcher/ to write the image to a MicroSD card (sufficiently large to hold the desired content, plus ~2GB for the OS and Kolibri).
1. Unplug and replug the MicroSD card into your computer to mount it, and write an empty file named "ssh" onto the "boot" partition.
1. Unmount and remove the MicroSD card, put it in your Raspberry Pi, and power it up. It will take some time to fully boot up.
1. Connect the Raspberry Pi to Ethernet. On a full-sized Raspberry Pi, this is easy as there's an Ethernet port. On a Pi Zero W, this requires a compatible Ethernet OTG MicroUSB adapter (e.g. https://smile.amazon.com/gp/product/B00RM3KXAU/), or you can attempt to do "Ethernet over USB" (http://www.circuitbasics.com/raspberry-pi-zero-ethernet-gadget/).
1. Determine the IP address of the Raspberry Pi on your network. You may be able to see this via your router's administration page (under something like "connected devices"), or you can hook the Pi up to a monitor and it will display the IP as part of the output during boot. Alternatively, you can run a scan using the tool "nmap" to search your network for devices with port 22 open: `nmap -p 22 192.168.0.1-254` (adapting the IP range to your network's subnet).
1. Create a Python virtualenv (optional), and then run `pip install -r requirements.txt`, to install Ansible.
1. Run `./install.sh <ip_of_raspberry_pi>` to initiate the installation process. It will ask for the SSH password, which is "raspberry" (username is pi, but it knows that already).
1. After it finishes running, you should be able to unplug the power from the Pi and plug it in again to restart it.
1. You should see a hotspot called "kolibri". Connect to it and put any domain name (no HTTPS) or the IP "10.10.10.10" to load Kolibri. If using a mobile device, you may need to disable your mobile data to ensure you're routed over the wifi to the Pi.

Things this script does:
- Installs Kolibri via the PPA/Debian package.
- Installs nginx and sets it up as a proxy for Kolibri on port 80, with caching.
- Moves the Kolibri content directory to /KOLIBRI_DATA/content, so that this SD card can be used for content import/export on other devices.
- Sets up hostapd and dnsmasq to make a wifi hotspot and turn it into a captive portal (directing all traffic to itself).
- Sets up a service to auto-mount USB media drives when plugged in.

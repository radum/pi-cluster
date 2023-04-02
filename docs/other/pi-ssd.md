# Use SSD with a Pi

## Resources

- [Raspberry Pi Cheap SSD Upgrade Guide 📰](https://jamesachambers.com/raspberry-pi-cheap-ssd-upgrade-30/)
- [Raspberry Pi 4 USB Boot is official! How-to 📺](https://www.youtube.com/watch?v=8tTFgrOCsig)
- [How to install Ubuntu Desktop on Raspberry Pi 4](https://discourse.ubuntu.com/t/how-to-install-ubuntu-desktop-on-raspberry-pi-4/18925)

> All 8GB Raspberry Pi 4s and newer 4GB models are pre-configured to boot from a USB drive automatically as long as there is no SD card inserted.

If you don't have one of those follow the steps bellow.

## Step 1 - Prepare the bootloader

Download Ubuntu Pi x64 from here https://ubuntu.com/download/raspberry-pi and flash it using Raspberry Pi Imager.

Check the settings before flashing to make sure the ssh keys and the name is correct.

```bash
cd ~

# Used to change the boot order
sudo apt install rpi-eeprom

# Extract the current bootloader configuration to a text file:
sudo vcgencmd bootloader_config > bootconf.txt

# Add BOOT_ORDER to the end of the previous file to tell the OS to try the USB first.
sed -i -e '/^BOOT_ORDER=/ s/=.*$/=0xf41/' bootconf.txt
# If the above doesn't work add it manually
# BOOT_ORDER=0xf41

# Now we generate a copy of the EEPROM with the update configuration:
# Check the paths are ok and there are no other files.
rpi-eeprom-config --out pieeprom-new.bin --config bootconf.txt /lib/firmware/raspberrypi/bootloader/critical/pieeprom-2020-09-03.bin

# Set the system to flash the new EEPROM firmware on the next boot
sudo rpi-eeprom-update -d -f ./pieeprom-new.bin

# To apply any changes (the EEPROM is only updated during the early stages of boot)
sudo reboot

# Once this is done and your USB SSD is in it should just boot from it.
```

## Useful commands

```bash
uname -a
uname -mrs
lsb_release -a
cat /etc/issue
hostnamectl
```

```bash
# How to Install Ubuntu Release 22.04

# Upgrade the server and ensure it is up to date.
sudo apt update -y && sudo apt upgrade -y && sudo apt dist-upgrade -y
# To simplify the upgrade, remove unused packages and files.
sudo apt autoremove -y && sudo apt autoclean -y
# Reboot the node to ensure any new kernel upgrades are installed.
sudo reboot

# Ensure the update-manager-core package is installed. On many systems, this package might already be available.
sudo apt install update-manager-core

# Confirm the release-upgrader is set to the correct release update mode. The file /etc/update-manager/release-upgrades must include the line Prompt=lts.
sudo cat /etc/update-manager/release-upgrades

# Run the do-release-upgrade command to start the upgrade.
sudo do-release-upgrade

# Ubuntu disables any third-party repositories during the upgrade. To search for disabled repositories, switch to the sources.list.d directory and list the entries.
cd /etc/apt/sources.list.d
ls -l

# Edit each list using a text editor. Remove the # symbol at the start of the affected entries, and save the file. In the following example, remove the # symbol in front of deb [arch=amd64].
nano archive-application.list

# Update any third-party repositories and remove unnecessary packages using apt commands.
sudo apt update -y && sudo apt upgrade -y && sudo apt dist-upgrade -y && sudo apt autoremove -y && sudo apt autoclean -y
```

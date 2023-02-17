# Ubuntu

## Downloads the Flash tool or use `Raspberry Pi Imager`.

> **Note:** All these commands are run from your computer, not the RPi.

```bash
sudo curl -L "https://github.com/hypriot/flash/releases/download/2.7.2/flash" -o /usr/local/bin/flash
sudo chmod +x /usr/local/bin/flash
```

## Download and extract the image

> **Note:** All these commands are run from your computer, not the RPi.

Is you use Raspberry Pi Imager you don't need to extract

```bash
cd ~/Downloads

curl -L "http://cdimage.ubuntu.com/releases/20.04/release/ubuntu-20.04.4-preinstalled-server-arm64+raspi.img.xz" -o ubuntu-20.04.4-preinstalled-server-arm64+raspi.img.xz
unxz -T 0 ~/Downloads/ubuntu-20.04.4-preinstalled-server-arm64+raspi.img.xz

# In case the above fails because of this https://github.com/rancher/k3s/issues/1712
# curl -L "http://cdimage.ubuntu.com/releases/eoan/release/ubuntu-19.10.1-preinstalled-server-arm64+raspi3.img.xz" -o ubuntu-19.10.1-preinstalled-server-arm64+raspi3.img.xz
# unxz -T 0 ~/Downloads/ubuntu-19.10.1-preinstalled-server-arm64+raspi3.img.xz
```

## Configure (skip if you use Raspberry Pi Imager)

> **Note:** All these commands are run from your computer, not the RPi.

Update [cloud-config.example.yml](../setup/cloud-config.example.yml) as you see fit.

## Flash (skip if you use Raspberry Pi Imager)

> **Note:** All these commands are run from your computer, not the RPi.

> **Note:** To find what /dev/disk number your flash drive has you can use `diskutil list`

```bash
# Check above `k3s/issues/1712`
# flash \
#     --userdata setup/cloud-config.example.yml \
#     ~/Downloads/ubuntu-20.04.2-preinstalled-server-arm64+raspi.img

flash \
    --userdata setup/cloud-config.example.yml \
    ~/Downloads/ubuntu-19.10.1-preinstalled-server-arm64+raspi3.img
```

If the utility says there is no SD card

```
# Lists all available devices, and pick the one you need
diskutil list

# Then pass another param to flash like this:

flash --userdata temp/cloud-config.worker3.yml --device /dev/disk2  temp/2022-01-28-raspios-bullseye-arm64-lite.img
```

## Boot

Place the SD Card in your RPi and give the system 5 minutes to boot before trying to SSH in.

```bash
ssh pi@192.168.0.100
```

## Install linux-modules-extra-raspi and other tools

If using Ubuntu 21 we need to install these:

```shell
sudo apt install linux-modules-extra-raspi && reboot
```

- https://github.com/k3s-io/k3s/issues/4234

## Upgrade EOL (Ubuntu 21 to 22)

- https://serverfault.com/questions/1106691/how-to-upgrade-ubuntu-21-10-to-22-04-after-eol

## Troubleshooting

### iptables is not working causing all kinds of issues (PODS not starting)

More info here https://bugs.launchpad.net/ubuntu/+source/linux-raspi/+bug/1962053

```bash
# To know which kernel versions where available, I used.
dpkg --list | grep linux-image
# Run this to bring back all modules after you run linux-modules-extra-raspi above and ansible
sudo apt install --reinstall linux-modules-$(uname -r)
# then iptables -V will be fine but on reboot they might fail again.
# now you can mount the NFS share folder and continue setting the K3S
sudo mount /mnt/ssd/
```

## TEMP

```bash
update-alternatives --set iptables /usr/sbin/iptables-legacy
update-alternatives --set ip6tables /usr/sbin/ip6tables-legacy
update-alternatives --set arptables /usr/sbin/arptables-legacy
update-alternatives --set ebtables /usr/sbin/ebtables-legacy
sudo modprobe ip_tables
sudo echo 'ip_tables' >> /etc/modules
sudo reboot
```

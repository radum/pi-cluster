# Ubuntu

> **Note:** All these commands are run from your computer, not the RPi.

## Downloads the Flash tool

```bash
sudo curl -L "https://github.com/hypriot/flash/releases/download/2.7.1/flash" -o /usr/local/bin/flash
sudo chmod +x /usr/local/bin/flash
```

## Download and extract the image

```bash
cd ~/Downloads

curl -L "http://cdimage.ubuntu.com/releases/20.04/release/ubuntu-20.04.2-preinstalled-server-arm64+raspi.img.xz" -o ubuntu-20.04.2-preinstalled-server-arm64+raspi.img.xz
unxz -T 0 ~/Downloads/ubuntu-20.04.2-preinstalled-server-arm64+raspi.img.xz

# In case the above fails because of this https://github.com/rancher/k3s/issues/1712
# curl -L "http://cdimage.ubuntu.com/releases/eoan/release/ubuntu-19.10.1-preinstalled-server-arm64+raspi3.img.xz" -o ubuntu-19.10.1-preinstalled-server-arm64+raspi3.img.xz
# unxz -T 0 ~/Downloads/ubuntu-19.10.1-preinstalled-server-arm64+raspi3.img.xz
```

## Configure

Update [cloud-config.example.yml](../setup/cloud-config.example.yml) as you see fit.

## Flash

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

## Boot

Place the SD Card in your RPi and give the system 5 minutes to boot before trying to SSH in.

```bash
ssh pi@192.168.0.100
```

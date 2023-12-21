# Ansible

> All these commands are run from your computer, not the RPi.

## Copy and update inventory and vars.yml

```bash
# Update inventory with your RPi IP addresses and hostnames
cp ansible/inventory.example ansible/inventory
# Update ansible_user to the user you used when you flashed Ubuntu
cp ansible/vars.example.yml ansible/vars.yml
```

## 1. Check RPis are ready to run accept Ansible

> **Note**: Prefix with watch command to view realtime

```bash
# Check they are connectable
env ANSIBLE_CONFIG=ansible/ansible.cfg ansible \
    -i ansible/inventory \
    kube_cluster -m ping

# Check that sudo is not blocked, if this return value is not 0 wait until it is
env ANSIBLE_CONFIG=ansible/ansible.cfg ansible \
    all -i ansible/inventory -m shell -a "test /var/lib/dpkg/lock-frontend && echo \$?"
```

## 2. Run playbook

> **Note**: Run this when all RPis are online

```bash
env ANSIBLE_CONFIG=ansible/ansible.cfg ansible-playbook \
    -i ansible/inventory \
    ansible/playbook.yml
```

## 3. Provision USB drive (OPTIONAL)

> **Note**: Run this when all RPis are online
>
> **Important**: Running this requires a USB drive inserted into each Pi, this playbook will format the ENTIRE flash storage

```bash
env ANSIBLE_CONFIG=ansible/ansible.cfg ansible-playbook \
    -i ansible/inventory \
    ansible/playbook-usbdrive.yml
```

## Useful commands

### Run the playboks only for certain machines

```bash
env ANSIBLE_CONFIG=ansible/ansible.cfg ansible-playbook -i ansible/inventory --limit 192.168.0.102 ansible/playbook.yml
```

### Check Temp of all RPis

> **Note**: This should be below 70.0'C for good performance

```bash
# For Raspberry OS
env ANSIBLE_CONFIG=ansible/ansible.cfg ansible \
    all -i ansible/inventory -m shell -a "/opt/vc/bin/vcgencmd measure_temp"

# For Raspberry Ubuntu
env ANSIBLE_CONFIG=ansible/ansible.cfg ansible \
    all -i ansible/inventory -m shell -a "cat /sys/class/thermal/thermal_zone*/temp | sed 's/\(.\)..$/.\1°C/'"
```

### Check overclock value of all RPis

> **Note**: This should be 175000

```bash
env ANSIBLE_CONFIG=ansible/ansible.cfg ansible \
    all -i ansible/inventory -m shell -a "cat /sys/devices/system/cpu/cpu0/cpufreq/cpuinfo_max_freq"
```

### Reboot the RPis

```bash
env ANSIBLE_CONFIG=ansible/ansible.cfg ansible -b \
    all -i ansible/inventory -m shell -a "/sbin/shutdown -r now"
```

### Shutdown the RPis

```bash
env ANSIBLE_CONFIG=ansible/ansible.cfg ansible -b \
    all -i ansible/inventory -m shell -a "/sbin/shutdown -h now"
```

### Uninstall k3s

```bash
# master and workers
env ANSIBLE_CONFIG=ansible/ansible.cfg ansible -b \
      all -i ansible/inventory -m shell -a "/usr/local/bin/k3s-killall.sh"

# master
env ANSIBLE_CONFIG=ansible/ansible.cfg ansible -b \
      all -i ansible/inventory -m shell -a "/usr/local/bin/k3s-uninstall.sh"

# workers
env ANSIBLE_CONFIG=ansible/ansible.cfg ansible -b \
      all -i ansible/inventory -m shell -a "/usr/local/bin/k3s-agent-uninstall.sh"
```

# SSD checks
```
pacman -Sy nvme-cli
nvme list
nvme smart-log /dev/nvme0
nvme self-test-log /dev/nvme0

# start extended check
nvme device-self-test /dev/nvme0 -n 1 -s 2
# get status with 
nvme self-test-log /dev/nvme0
```


## trim ssd
```
pacman -Sy util-linux
systemctl enable fstrim.timer
# trim manually
fstrim -v --fstab
```

## support trim on crypted device
```
GRUB_CMDLINE_LINUX="cryptdevice=/dev/nvme0n1p3:cryptroot:allow-discards"
```

# rp4
Cheetsheat install guide ArchLinux Raspberry Pi 4

## Init from PC
Insert SD card into the computer.
```bash
sudo fdisk -l
sudo fdisk /dev/sdX
o
n
p
1
2048
+200M
t
c
n
p
2
default
default
w
sudo mkfs.vfat /dev/sdX1
mkdir tmp
cd tmp
mkdir boot
sudo mount /dev/sdX1 boot
sudo mkfs.ext4 /dev/sdX2
mkdir root
sudo mount /dev/sdX2 root
wget http://os.archlinuxarm.org/os/ArchLinuxARM-rpi-aarch64-latest.tar.gz
su
bsdtar -xpf ArchLinuxARM-rpi-aarch64-latest.tar.gz -C root
sync
mv root/boot/* boot
sed -i 's/mmcblk0/mmcblk1/g' root/etc/fstab
umount boot root
exit
cd
sudo rm -r tmp
```

## From Raspberry Pi 4
- Insert the SD card into the Raspberry Pi.
- Connect ethernet.
- Apply 5V power.
- Use the serial console or SSH to the IP address given to the board by your router.
- Default user `alarm` with the password `alarm`.
- The default `root` password is `root`.

### Example
Example via SSH from PC:
```bash
ssh alarm@alarm
su
```
Initialize the pacman keyring and populate the Arch Linux ARM package signing keys:
```bash
pacman-key --init
pacman-key --populate archlinuxarm
```
Update system and install `sudo`
```bash
pacman -Syu
pacman -S sudo
vim /etc/sudoers
```
Uncomment
```python
%wheel ALL=(ALL) ALL
```
Change passwords
```bash
passwd
passwd alarm
exit
```

## Some apps
```bash
sudo pacman -S gvim git
```

## Mount usb to /home directory
Insert usb device
```bash
sudo fdisk -l
sudo fdisk /dev/sdX
o
n
default
default
default
w
sudo mkfs.ext4 /dev/sdX1
sudo mkdir /dev1
sudo mount /dev/sdX1 /dev1
sudo rsync -r -P ../../home/* ../../dev1
sudo cd /
sudo rm -r home/*
sudo umount /dev/sdX1
sudo mount /dev/sdX1 /home
sudo chown -R alarm:users /home/alarm
sudo chown -R alarm:users /home/alarm/.[^.]*
ls -l /dev/disk/by-uuid/
sudo vim /etc/fstab
```
```python
#Static information about the filesystems.
# See fstab(5) for details.

# <file system> <dir> <type> <options> <dump> <pass>
/dev/mmcblk1p1  /boot   vfat    defaults        0       0

UUID=enter-your-uuid-code       /home           ext4            rw,relatime
 0 1
```
```bash
sudo reboot
```

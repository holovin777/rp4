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
ssh alarm@192.168.1.XXX
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
sudo pacman -S gvim
```

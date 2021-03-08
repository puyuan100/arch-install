# arch-install
Here I want to introduce how to install Arch linux. And this is prepared for myself.

## set time
```
# timedatectl set-ntp true
```
## The partition and mount 
```
# fdisk /dev/sda
# g
# +512M for EFI, +2G for /boot ,+8G for swap, +40G for /, rest for /home
# w

# mkfs.ext4 /.../... , this is for /boot,/home,/
# mkswap /.../..., this is for swap
# mkfs.fat -F32 /.../..., this is for EFI

# mount /.../... /mnt
# mkdir /mnt/boot
# mkdir /mnt/boot/EFI
# mkdir /mnt/home
# mount /.../... /mnt/boot
# mount /.../... /mnt/boot/EFI
# mount /.../... /mnt/home
# swapon /.../...
```
## installation
```
# pacstrap /mnt base linux linux-firmware vi vim
```

## Configure the system
```
# genfstab -U /mnt >> /mnt/etc/fstab
# arch-chroot /mnt
# ln -sf /usr/share/zoneinfo/Europe/Paris /etc/localtime
# hwclock --systohc 
# vim /etc/locale.gen (here to remove the "#" of "en_US.UTF-8 UTF-8")
# locale-gen
# vim /etc/locale.conf (here write "LANG=en_US.UTF-8")
# vim /etc/hostname (for example "arch")
# vim /etc/hosts ( 127.0.0.1 localhost \\ ::1 localhost \\ 127.0.1.1 hostname.localdomain hostname)
# passwd (creat the password for root)
# pacman -S grub efibootmgr networkmanager network-manager-applet wireless_tools wpa_supplicant dialog os-prober mtools dosfstools base-devel linux-headers dhcpcd
# grub-install --target=x86_64-efi --efi-directory=/boot/EFI --bootloader-id=GRUB
# grub-mkconfig -o /boot/grub/grub.cfg
# exit
# umount -a
# reboot
```
## Then login to do futher config
```
# dhcpcd (this aims to link internet)
# useradd -m -G wheel haha (haha is an example)
# passwd haha
# EDITOR=vim visudo (to remove the "#" of the sentence "%wheel ALL=(ALL) ALL")
```

## instrall the Desktop

### kde
```
# pacman -S xf86-video-intel (or xf86-video-amdgpu for AMD)(nvidia nvidia-utils for Nvidia)
# pacman -S xorg
# pacman -S sddm
# systemctl enable sddm
# pacman -S plasma kde-applications xdg-user-dirs packagekit-qt5
```
### xfce 
```
# pacman -S xf86-video-intel
# pacman -S xorg
# pacman -S xfce4 xfce4-goodies
# pacman -S lightdm lightdm-gtk-greeter lightdm-gtk-greeter-settings
# systemctl enable lightdm
```


## for mac (wifi broadcom wireless)
```
# rmmod b43
# rmmod ssb
# sudo pacman -S broadcom-wl-dkms
# modprobe wl

```

## for VMware
```
# sudo pacman -S open-vm-tools
# sudo pacman -S gtkmm
# sudo pacman -S xf86-video-vmware
# sudo pacman -S xf86-input-vmmouse
# systemctl enable vmtoolsd
```


# arch-install
Here I want to introduce how to install Arch linux. And this is prepared for myself.

## set time
```
# timedatectl set-ntp true
```
## The partition and mount 
```
"Connect wifi"
# ip link (find the name of the device, eg wlan0)
# rkfill unblock all
# ip link set wlan0 up
# iwctl
# device list
# station wlan0 scan
# station wlan0 get-networks
# station wlan0 connect wifi_name
# (then input the password)
# exit
# (done)


# fdisk /dev/sda
# g
# +512M for EFI, +2G for /boot ,+8G for swap, +40G for /, rest for /home
# w
# t (set EFI and SWAP)

# mkfs.fat -F32 /.../..., this is for EFI
# mkswap /.../..., this is for swap
# mkfs.ext4 /.../..., this is for /, /home, /boot


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
# pacstrap /mnt base base-devel linux linux-firmware linux-headers intel-ucode ntfs-3g vi vim
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

### gnome
```
# pacman -S xf86-video-intel nvidia
# pacman -S xorg
# pacman -S gdm
# pacman -S gnome
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

# Config

## zsh
```
# sudo pacman -S zsh
# chsh -s /bin/zsh
# yay -S oh-my-zsh-git
# cp /usr/share/oh-my-zsh/zshrc ~/.zshrc
# yay -S zsh-syntax-highlighting zsh-autosuggestions
# sudo vim ~/.zshrc
  plugins=(
  git
  autojump
  zsh-syntax-highlighting
  zsh-autosuggestions
  )
# sudo ln -s /usr/share/zsh/plugins/zsh-syntax-highlighting /usr/share/oh-my-zsh/custom/plugins/
# sudo ln -s /usr/share/zsh/plugins/zsh-autosuggestions /usr/share/oh-my-zsh/custom/plugins/

```

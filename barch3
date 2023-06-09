#!/bin/bash

# Arch Linux installatie script met FVWM-Crystal, SSH en RDP

# Partitie maken en formatteren
# Vervang /dev/sdaX door de gewenste partitie
mkfs.ext4 /dev/sdaX
mount /dev/sdaX /mnt

# Basisinstallatie
pacstrap /mnt base base-devel

# Fstab genereren
genfstab -U /mnt >> /mnt/etc/fstab

# Chroot naar het nieuwe systeem
arch-chroot /mnt /bin/bash <<EOF

# Tijdzone instellen
ln -sf /usr/share/zoneinfo/Europe/Amsterdam /etc/localtime
hwclock --systohc

# Taalinstellingen
echo "nl_NL.UTF-8 UTF-8" > /etc/locale.gen
locale-gen
echo "LANG=nl_NL.UTF-8" > /etc/locale.conf

# Hostnaam instellen
echo "arch" > /etc/hostname

# Netwerkconfiguratie
echo "127.0.0.1    localhost" >> /etc/hosts
echo "::1          localhost" >> /etc/hosts
echo "127.0.1.1    arch.localdomain  arch" >> /etc/hosts

# Root wachtwoord instellen
echo "Root wachtwoord instellen:"
passwd

# SSH installeren en inschakelen
pacman -S openssh
systemctl enable sshd.service

# Xorg en FVWM-Crystal installeren
pacman -S xorg fvwm-crystal

# LightDM Display Manager installeren en inschakelen
pacman -S lightdm lightdm-gtk-greeter
systemctl enable lightdm.service

# RDP (XRDP) installeren
pacman -S xrdp
systemctl enable xrdp.service

# RDP-poort openen in de firewall
iptables -A INPUT -p tcp --dport 3389 -j ACCEPT

# Sluit de chroot-omgeving af
exit
EOF

# Systeem opnieuw opstarten
umount -R /mnt
reboot

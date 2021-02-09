+++
title = "Artix Linux Installation"
date = "2020-01-01"
tags = ["random"]
categories = ["Category 1"]
description = "One of my new years resolution was to try a different a linux distro, but little did i know..."
+++
> This article is a test...
## Why?
That was my initial question and thought, but ultimately it became why not? Not long ago, i decided to ditch the world of Windows & macOS in the year 2016. I breathed and transpired the same air as the Xenial Xerus (Ubuntu 16.04). From that point onwards, i distro hopped multiple times in virtualbox and on my physical machine in search of something. I didn't know what i wanted exactly, but thats what i did. Throughout my distrohopping i found a common theme that i was unaware off. Every distribution i installed had a graphical installer and came preconfingured with a bloated desktop environment. I never knew why, but i certainly knew that no matter what distribution i used, i could never truly get what i desired, which i later discovered was speed. I researched extensively and finally stumbled upon something called Arch Linux. At first i was scared because the idea of doing everything yourself felt exhilirating. After reading about the core principles, simplicity sticked out to me. This entire time i was using bloated distros that ended up hogging so much memory that i totally forgot that simplicity is the key to speed. I was missing the third s.I looked up videos on how to install arch linux and i was immediately drawn away. I tried looking for alternatives to arch linux. I wanted the arch linux experience, but without the seaminly harsh installation process. I liked Manjaro, but that was still bloated, even the communit editions. By some oddity, i ended reading about this thing called systemd. Finally i found the third s, systemd-free . Artix linux was the first thing i stumbled upon and i was all in. Simplcity, speed and systemd -free, the 3 s's of well informed decision. Luckily enough there was an installation video on YouTube, but i took notes. This is the process by which i installed the linux distribution which i have finally settled down on.

## Full Installation

User:root
Password:artix

> Unblock WiFi
<span style='color:#a8ff17;'>

`rfkill unblock wifi`

</span>

> Wlan0  - WiFi card/adapter. Eth0  - Ethernet. Setup wlan0

`ip link set wlan0 up`

> WiFi utility

`connmanctl`

> Scan networks

`scan wifi`

 See list of connections

`services`

> Turn on agent

`agent on`

> Connect to network

`connect wifi_x_x_managed_psk`

> Exit WiFi utility

`quit`

> Check connection. Ctrl + C to exit ping.

`ping website` or `ip a`


> Check the disk

`lsblk`

> Partition the disk

`cfdisk /dev/sda`

> Press delete key until you see free space

`new` > `500M` > `type=EFI System` > `write` > `yes`

`new` > `enter` > `type=Linux Filesystem` > `write` > `yes` > `quit`

>  Make a fat partition on the EFI System

`mkfs.fat -F32 /dev/sda1`

> Make an Ext4 partition on Linux filesytem

`mkfs.ext4 /dev/sda2`

> Check partitions again

`lsblk`

> Mount root partition

`mount /dev/sda2  /mnt`

> Make an EFI boot directory in the root partition

`mkdir -p /mnt/boot/efi`

> Mount boot partition onto the EFI system

`mount /dev/sda1  /mnt/boot/efi`

> Check to see the mounted partitions

`lsblk`

> Install the base system

`basestrap  /mnt  linux linux-firmware base base-devel runit elogind-runit intel-ucode nano`

> Fstab

`fstabgen -U /mnt >> /mnt/etc/fstab`

> Check the fstab

`cat /mnt/etc/fstab`

> Move from the installer into the iso

`artix-chroot  /mnt`

> For a more interactive experience, switch the shell

`bash`

> Create a swap file

`dd if=/dev/zero of=/swapfile  bs=1G count=2 status=progress`

> Change the permissions of the swap file

`chmod 600 /swapfile`

> Make the swap partition

`mkswap /swapfile`

> Mount the swap partition

`swapon /dev/swapfile`

> Add the swap partition into the fstab

`nano /etc/fstab`

/swapfile     none   swap   defaults   0  0

> Change the time

`ln -sf /usr/share/zoneinfo/America/Chicago  /etc/localtime`

> Sync system clock with your hardware

`hwclock  --systohc`

> Select your locale

`nano /etc/locale.gen`

> Uncomment

`en_US.UTF-8  UTF-8`

> Generate your locale

`locale -gen`

> Add the locale to the configuration

`nano /etc/locale.conf`

`LANG=en_US. UTF-8`

> Create a hostname

`nano /etc/hostname`

`artix`

> Create a hosts file

`nano /etc/hosts`

127.0.0.1     localhost
::1                  localhost
127.0.1.1      artix.localdomain   artix

> Change the root password

`passwd`

> Install grub & other packages

`pacman -S grub efibootmgr networkmanager networkmanager-runit network-manager-applet dosfstools linux-headers xdg-utils xdg-user-dirs`

> Grub install

`grub-install --target=x86_64-efi --efi-directory=/boot --bootloader-id=grub `

> Configure grub

`grub-mkconfig -o /boot/grub/grub.cfg`

> Add a user

`useradd -m user`

> Add password for user

`passwd user`

> Change the sudoers  file

`EDITOR=nano visudo`


> Exit the chroot environment

`exit` & `exit`

> Unmount  system

`umount -R /mnt `

> Reboot system & Remove installation media

`reboot`

</details>

<details><summary>Post-Installation</summary>

`yay -S pixelorama-bin helm-synth godot vital-synth gimp-git inkscape-git audacity-git odin2-synthesizer aseprite alacritty android-tools reaper-bin vscodium-bin awesome librewolf-bin calcurse alacritty pandoc vlc-git pulseaudio-gitit thunar libreoffice-still unzip ranger lmms-beta-bin flutter android-file-transer nvidia nvidia-utils neofetch mlocate updatedb calibre-git picom feh polybar rofi volctl vim redshift font-manager gnome-tweaks-git sweet-theme-git papirus-icon-theme-git rsm oomox lxappearance-gtk3 beaker tor-browser hugo pngcrush oxipng optipng birdfont fontforge blender dust3d makehuman goxel gpick lilypond vcvrack element fluidsynth carla mc tiled htop sdcv stardict gnupg ccrypt sirikali carp encfsui cryptkeeper cava ttf-twemoji-color ttf-bitstream-vera zip unarchiver p7zip `

[Mount Android Device Guide](https://techsner.com/2020/08/05/how-to-mount-android-phone-on-arch-linux/)

[Install Yay AUR Helper](https://www.tecmint.com/install-yay-aur-helper-in-arch-linux-and-manjaro/)


<!-- Librewolf is my new favorite browser of choice and dare i say faster. Faster than Brave browser even. My DNS setup with instruction found [here(https://support.opendns.com/hc/en-us/articles/360038086532-Using-DNS-over-HTTPS-DoH-with-OpenDNS or https://developers.cloudflare.com/1.1.1.1/1.1.1.1-for-families/setup-instructions/dns-over-https )]  -->


</details>

<details><summary>Final-Touches</summary>

- Brightness Control <!-- Brightness control (ACPI). The most efficient way to edit brightness: echo # > /sys/class/backlight/nvidia_0/brightness. Even better set an alias. like eb 50 = 50%, eb 25 =25% The problem with the Nvidia Gpu is the fact that that the nvidia_0 values do not reflect that of the ones set using the xbacklight -set # command. They function independently. The brightness can also be adjusted through the Nvidia X settings. I need to reread this [page](https://wiki.archlinux.org/index.php/Backlight)-->
- Volume Control <!--Get the volume to work (ALSA Jack,Pulseaduio ) - You have a sof card, install the sof-firmware and alsa-ucm-conf packages. Link [here](https://bbs.archlinux.org/viewtopic.php?id=262469) Alsa utils uses systemd. More about Alsa [here](https://wiki.archlinux.org/index.php/Advanced_Linux_Sound_Architecture) -->
- Android MTP <!-- Android MTP (file transfer) I do miss drag and and drop, but aft-mtp-cli saved me. The comamand get [file or directort] [LinuxFS directory] . Super cool. I just transfered 2.5GB from my phone to PC in 16 seconds. That would have taken minutes with a GUI application Mounting external media (Flash drives,portable storage etc)  -->
- Yubico Authentication <!-- Systemd problem. Read the Void linux documentation to find a solution on how to enable Pcscd on the Runit init system -->
- Multilingual Support <!-- Locale/Localization(L10n)/Internalization(i18n) Problem: Unable to displays characters other than en_US.UTF-8 (Only roman/latin characters, No CJKV characters; chinese, japanese, korean,vietnamese) (Not multilingual, especially in browsers) (Is it a font problem?) Large font collection package:pacman -S adobe-source-han-sans-otc-fonts. That did the trick, just a simple font installation    --> 
- VST Functionality <!-- Most annoying problem by far, the VST's were able to function flawlessly on Manjaro, but the DIY lifestyle has some things that make you wish you had just sticked to Manjaro.I cant get my VST's most of my VST's to work on my setup, so i went ahead and tried something experimental. I installed the entire pro-audio group of applications which consists of 180 packages tantamounting to nearly 4GB. I just need a working solution and then i'll slowly remove each package. Overkill is better than having to search for a solution when there isnt any Zynaddsubfx sound not working. Possible solution [here](https://bbs.archlinux.org/viewtopic.php?id=227838) -->

</details>

<details><summary>Customization</summary>

- Alacritty <!--Copy the config file to the home directory. Uncomment the theme. Color format Hex (0x2c2c2c) . Just made everything green --> 
- VScodium <!--(Just black). To make the menu bar match change from native to custom on (window.titleBarStyle) -->
- Aseprite <!-- Install a .extension or .zip as an extensions and click apply in themes under preferences.You can also manually theme by editing the theme.xml located in /usr/share/aseprite/data/extensions/aseprite-theme -->
-Librewolf <!-- Mozilla has killed Firefox in the same manner that Brave browser did with its affilitate links scandal. Librewolf is a fork of Firefox that ships without Telemetry and has better privacy hardening features. Firefox configuration [here](https://privacytools.io/browsers/#about_config) Theme configuration [here](https://github.com/manilarome/blurredfox/).Ferdi was great, but at the end of the day you only need a single browser, even if that browsr is optimized for social networking sites and email. -->
- GRUB Theme <!-- [here](https://www.gnome-look.org/p/1429443/) -->
- GTK3 Theme <!-- Sweet-Dark. Theme from [Pling](https://www.pling.com/s/Gnome/p/1253385/) . 2 simplww commands to set it. gsettings set org.gnome.desktop.interface gtk-theme Sweet ... gsettings set org.gnome.desktop.wm.preferences theme Sweet -->
- Reaper <!-- VST is stored here /usr/lib/vst and here /usr/lib/vst3 -->
- Awesomewm [option 1](https://github.com/manilarome/the-glorious-dotfiles) [Option 2](https://github.com/WillPower3309/awesome-dotfiles)
- Compositor (Kawase-blur) <!-- i am currently unable to use picom due to some errors. I'll search for a solution at a later time. There is no need to spend hours trying to solve a problem that doenst hinder my workflow. I would love to have that kawase blur, but man must take break and do other things -->
- [Oomox](https://github.com/themix-project/oomox)

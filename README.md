# Linux Quick Reference

This document is based on my last few years of using Linux Mint (and somewhat less so Ubuntu), and, perhaps even to a bigger extent, on the articles describing advanced Linux commands that I have been reading as of late. The main goal here was to create a reference containing the most important pieces of information (e.g. terminal commands) that are hard to keep in mind all the time, but that come in handy every now and then while working with Linux, and especially when one needs to install Linux from scratch. Also, please note that even though I'm a software engineer, and therefore I am mentioning here certain developer tools, like Docker or IntelliJ, that aren't necessarily relevant for an average or even advanced Linux user.

## Commands

I assume most Linux users are probably aware of the following commands, and hence I won't be touching on them in this reference: `sudo`, `su`, `pwd`, `ls`, `cd`, `mkdir`, `mv`, `cp`, `rm`, `touch`, `echo`, `cat`, `grep`, `less`, `view`, `vim`. If you don't know what they do, make sure you to check them out before proceeding further (you can find a good link or two in the end of this document).

##### Getting Help
- `man bash` - show the manual page for a command 
- `tldr tar` - show examples of executing a commmand

##### System Information
- `inxi -F` - print the detailed system / hardware info
- `whoami` - return the current user name
- `uname -a` - print out some basic system info (e.g. Kernel version)
- `uptime` - show the time the system was up and load average
- `lsusb` - list USB devices
- `lspci` - list PCI devices
- `env` - prints all environment variables
- `history` - print the history of executed commands

##### System Monitoring
- `vmstat 1` - print out (every second) the resource utilization in a tabular format
- `free` - display the amount of free and used memory
- `top` - show linux processes, cpu utilization, memory usage etc
- `htop` - an "improved" version of `top`
- `s-tui` - show graphs of freq, utilization and temprature of CPU, as well as the power usage
- `sudo powertop` - power consumption / power management tool
- `atop` - view the load on the system 
- `iostat 1` - report (every second) read/write and other statistics for devices and partitions 

##### Process / System Management
- `ps aux | grep java` - show running java processes
- `pstree` - show running prcesses as a tree
- `kill process_id` - send a signal for the process to terminate
- `crontab -l` - list (edit) cron jobs of the current user 
- `reboot` - signal system to reboot
- `poweroff` - signal system to shutdown

##### Troubleshooting
- `dmesg --level err,warn` - show errors/warning in the kernel ring buffer
- `sudo service --status-all` - show status of all services
- `sudo systemctl start/stop/restart/reload docker` - start/stop/restart/reload a systemd unit (e.g. docker daemon)
- `journalctl -b` - show all journald messages from this boot 
- `strace -f script.py` - trace system calls
- `man 2 open` - shows a manual for a system call

##### Package Management
- `dpkg -l` - lists all installed packages
- `dpkg -S /usr/bin/ab` - shows the package owning a file
- `apt search linux-image-*` - searches repos for a package
- `apt policy linux-image-4.13.0-45` - shows repositories that contain the package

##### Files
- `whereis ls` - locate the binary, source, man page for a command
- `ln -s file symlink` - create a symbolic link to a file / folder
- `md5sum file.tar.gz` - calculate MD5 checksum
- `find ~/Document -name *.sh` - find files under the given directory
- `tar xzf source.tar.gz` - extract a gzipped archive in the current directory
- `zip -r my.zip ~/Documents` - package and compress a directory as zip file
- `sed -i -e 's/find/replace/g' filename` - replaces a string in a file
- `perl -p -i -e 's/find/replace/g' filenames` - replaces a string in one or more files

##### Users & Groups
- `passwd` - change password of the current user
- `useradd`, `userdel`, `su`, `sudo`
- `w` and `who`
- `chmod +x script.py` - make a file executable by everyone
- `chown ubuntu:ubuntu ~/Applications` - change ownership of a file / directory

##### Disks, Partitions, Mounts
- `lsblk` - list block devices (e.g. disk partitions and loopback devices)
- `df -h` - report disk space usage
- `du -sh ~/Documents` - estimate file / directory space usage  
- `dd if=file.iso of=/dev/usb_drive status=progress` - make a bootable usb drive from an isohybrid file 
- `findmnt` - list (or search in) all mounted file systems
- `mount /dev/sdb2 /media/myusername/usb/` - mount a file system (e.g. usb drive)
- `umount /dev/sdb*` - unmount a file system (e.g. usb drive)
- `mkfs.ext4`
- `sudo fdisk -l`
- `sudo parted -l`
- `rsync` - sync files to / from a remote host

##### SSH & HTTP
- `ssh`
- `scp`
- `wget`
- `curl`
- `ab`
- `siege`

##### Networking
- `nmcli` - a cli tool for controlling NetworkManager
- `ping 8.8.8.8` - test connectity between host and provided IP 
- `traceroute 8.8.8.8` or `tracepath 8.8.8.8` - trace packets route to a host
- `dig` or `nslookup` - DNS lookup utilities 
- `ip address` (formerly `ifconfig`) - show / manipulate network interfaces
- `ip route` (formerly `route`) - show / manipulate the IP routing table
- `ip link set eth0 up/down` (`ifup`/`ifdown`) - enable / disable a network interface
- `ss -a` (formerly `netstat`) - list open sockets
- `iw` (formerly `ifconfig`) - show / manipulate wireless devices 
- `tcpdump -i eth0 port 80` - capture traffic off a network interface
- `tcpflow -c -i eth0 port 80` - capture and save traffic for analysis / debugging
- `nmap` - network exploration tool / port scanner
- `nc` (or more versatile `socat`) - listen on / connect to ports, forward data etc. 

##### Shell Goodies
- `alias l='ls -l'` - create an alias for a command
- `jobs`, `fg`, `bg`,
- `nohup`
- `time`
- `watch`
- `tee`
- `jq`
- `wc`
- `awk`
- `xargs`
- `head`
- `tail`

## Recipies

### Creating a bootable USB stick from `.iso` file
- `umount /dev/sdb*`
- `sudo fdisk –l` and figure out how the USB drive is called (e.g. `/dev/sdb`)
- `mkfs.vfat /dev/sdb –I`
- `dd if=~/Documents/ISOs/linuxmint-19-cinnamon-64bit-beta.iso of=/dev/sdb status=progress`

## Dotfiles

TODO cover dotfiles

- `.bash_aliases`
- `.bash_logout`
- `.bash_profile`
- `.bashrc`
- `.profile`
- `.inputrc`
- `.selected_editor`
- `.npmrc`
- `.gitconfig`
- `.tmux.cof`

## Filesystem

TODO cover most useful/important paths in the filesystem

- `/etc/apt`
- `/var/log`
- `/usr/lib/jvm/`
- `~/.xsession-errors`
- `~/.local/share/applications/`

## Repos / PPAs

In order to be able to install and/or get the latest version of the following software, add the repos mentioned below:  

- TLP
```
sudo add-apt-repository ppa:linrunner/tlp
```
- Yubikey Tools
```
sudo add-apt-repository ppa:yubico/stable
```
- Vivaldi
```
echo "deb http://repo.vivaldi.com/stable/deb/ stable main" | sudo tee /etc/apt/sources.list.d/vivaldi.list > /dev/null
wget -q -O - http://repo.vivaldi.com/stable/linux_signing_key.pub | sudo apt-key add -
```
- Chrome
```
echo "deb [arch=amd64] http://dl.google.com/linux/chrome/deb/ stable main" | sudo tee /etc/apt/sources.list.d/google-chrome.list > /dev/null
wget -q -O - https://dl-ssl.google.com/linux/linux_signing_key.pub | sudo apt-key add -
```
Depending on your Linux distro, you might also want to add more repos to be able to use the latest versions of the following: Docker, NodeJS, Postgres etc.

## Software Checklist

- `apt install guake` - **Guake** - a dropdown terminal that can be shown / hidden with a shortcut
- `apt install tmux` - **Tmux**, a terminal multiplexer, e.g. allows splitting your terminal into panes
- `apt install vim-gnome` - **Vim**, a version of Vim that allows yanking/pasting to/from the clipboard (`"+y`/`"+p`) 
- `apt install docker.io` - **Docker**, software for running containers ~*i.e. the lightweight VMs*~
- `apt install vivaldi-stable` - **Vivaldi**, a browser that supports grouping multiple web pages in a single tab
- `apt install google-chrome-stable` - **Google Chrome**, a free web browser from Google, IMO a bit more stable than Chromium
- `apt install tlp tlp-rdw` - **TLP**, a Linux power management tool, must-have on laptops
- `apt install nodejs` - **Node.js**, a javascript runtime, brings **npm** on board
- `apt install snapd` - **Snappy**, a cross-distro package manager developed by Canonical
- `apt install yubioath-desktop yubikey-neo-manager yubikey-personalization-gui` - **Yubikey Authenticator**, **Yubikey Neo Manager**, **Yubikey Personalization Tool**, all needed, one way or another, in case if you are using Yubikeys
- `apt install virtualbox virtualbox-qt` - **VirtualBox**, software for running VMs
- `apt install vagrant` - **Vagrant**, a tool for managing development environments, requires VirtualBox or an alternative
- `apt install wireshark-qt` - **Wireshark**, a GUI for wireshark packet analyzer

- `snap install skype` - **Skype**, a text/voice/video chat app ~pwned~ owned by Microsoft
- `snap install slack` - **Slack**, a team colaboration tool, i.e. a text chat on steroids
- `snap install spotify` - **Spotify**, a music streaming app by Spotify
- `snap install intellij-idea-ultimate`- **Intellij IDEA**, possibly the best IDE for developing any app that is JVM-based, there is also a free community edition with less features 
- `snap install bitwarden` - **BitWarden**, an open source password manager done right (at least from the user perspective)

- `npm install -g tldr` - **TLDR**, a great collection of simplified man pages, first stop for help on any terminal command

Also: jq, ab, powertop, htop, s-tui, direnv, docker-compose, sdkman

## Useful Links
- [Bash shortcuts](https://github.com/fliptheweb/bash-shortcuts-cheat-sheet)
- [Vim shortcuts](https://vim.rtorr.com/)
- [Tmux shotcuts](https://gist.github.com/MohamedAlaa/2961058)
- [100 Useful Unit Commands](http://oliverelliott.org/article/computing/ref_unix/)
- [Deprecated Linux networking commands](https://dougvitale.wordpress.com/2011/12/21/deprecated-linux-networking-commands-and-their-replacements/)
- [Advanced Bash-Scripting Guide](https://www.tldp.org/LDP/abs/html/abs-guide.html)
- [Guide to using YubiKey as a SmartCard for GPG and SSH](https://github.com/drduh/YubiKey-Guide#export-public-key)

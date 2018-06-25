# Linux Mint via Terminal - Quick Reference

For the past couple of years I have been using Linux Mint as my main OS for software development as well as for some more basic stuff, like web-browsing and casual gaming. 
However, as I kept using Terminal more and more in my day-to-day tasks, I indeed started wondering as to what extent I can go with just command line alone and how I can easily replicate my local Linux set up on a new machine. 
This all resulted in me coming up with this document, which is meant to contain commands and other pieces of information that are hard to keep in mind all the time, but that come in handy every now and then while working with Linux. 
Additionally, I have also included a section that covers the software that I tend to install on any new laptop/desktop I will be working on.

## TODOs
- Consider using Flatpak instead of Snappy in the Software section

## Commands

I assume most Linux users are probably aware of the following commands, and hence I won't be touching on them in this reference: `sudo`, `su`, `pwd`, `ls`, `cd`, `mkdir`, `mv`, `cp`, `rm`, `touch`, `echo`, `cat`, `grep`, `less`, `view`, `vim`.
If you don't know what they do, make sure to check them out before proceeding further (if you need some learning materials, take a look at the links in the end of this document).

##### Getting Help
- `apropos docker` - show names / descriptions of man pages matching a keyword
- `man bash` - show the manual page for a command 
- `tldr tar` - show examples of using a commmand (find out more at [tldr.sh](http://tldr.sh/))

##### System Information
- `inxi -F` - print the detailed system / hardware info
- `whoami` - return the current user name
- `uname -a` - print out some basic system info (e.g. Kernel version)
- `uptime` - show the time the system was up and the load averages
- `date` - show the current date and time
- `lsusb` - list USB devices
- `lspci` - list PCI devices
- `lsblk` - list block devices (e.g. disk partitions and loopback devices)

##### System Monitoring
- `top` - show linux processes, cpu utilization, memory usage etc
- `htop` - an "improved" version of `top`
- `glances` - an alternative to `htop`, includes disk and network stats
- `vmstat 1` - print out (every second) the resource utilization in a tabular format
- `free` - display the amount of free and used memory
- `iostat 1` - report (every second) read/write and other statistics for devices and partitions
- `s-tui` - show graphs of frequency, utilization and temprature of CPU, as well as the power usage
- `sudo powertop` - power consumption / power management tool
- `stress -c 4` - run 4 CPU-loading workers (can also stress-test memory, io etc.)
- `sudo tlp-stat --battery` - show the battery charge and capacity

##### Process / System Management
- `ps aux | grep java` - show running java processes
- `pstree` - show running prcesses as a tree
- `kill pid` - send a signal for the process to terminate (or `kill -9 pid` to forcefully terminate the process)
- `crontab -l` - list (edit) cron jobs of the current user 
- `reboot` - signal system to reboot
- `poweroff` - send the ACPI signal to the system to power down (works great with *Alt+F2* in Cinnamon/Gnome)
- `halt` - terminate all processes and stop all the CPU functions (but leave it powered on)
- `shutdown -h now` - shut down the system now (similar to `poweroff`) 

##### Troubleshooting
- `dmesg --level err,warn` - show errors / warnings in the kernel ring buffer
- `sudo systemctl start|stop|restart|reload docker` - start/stop/restart/reload a systemd unit (e.g. docker daemon)
- `sudo service --status-all` - show status of all services (can also start/stop/restart/reload individual services)
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
- `tar xzf source.tar.gz` - extract a gzipped tarball in the current directory
- `gzip -d source.gz` - decompress a gzipped file
- `zip -r my.zip ~/Documents` - package and compress a directory as a zip file
- `sed -i -e 's/find/replace/g' filename` - replaces a string in a file
- `perl -p -i -e 's/find/replace/g' filenames` - replaces a string in one or more files

##### Users & Groups
- `id` - print current user and group IDs
- `sudo useradd john` - create a new user
- `sudo passwd john` - change password of the user (or just `passwd` for the current user)
- `sudo userdel -r john` - remove the user and their home directory
- `sudo groupadd visitor` - create a new group (`groupdel` to delete a group)
- `sudo adduser john visitor` - add the user to the group
- `w` and `who` - show who is logged in and their activity
- `chmod u+x script.py` - make a file executable by the user owning it
- `chown ubuntu:ubuntu ~/Applications` - change ownership of a file / directory
- `finger john` - show info about a user

##### Disks, Partitions, Mounts
- `df -h` - report disk space usage
- `du -sh ~/Documents` - estimate file / directory space usage  
- `dd if=file.iso of=/dev/usb_drive status=progress` - make a bootable usb drive from an isohybrid file 
- `sync` - write all pending writes on all disks (e.g. to make sure `dd` is finished)
- `sync; echo 1 > /proc/sys/vm/drop_caches` - clean PageCache (may come in handy when benchmarking)
- `findmnt` - list (or search in) all mounted file systems
- `mount /dev/sdb2 /media/myusername/usb/` - mount a file system (e.g. usb drive)
- `umount /dev/sdb*` - unmount a file system (e.g. usb drive)
- `mkfs.ext4 /dev/sdb2` (and bunch of other `mkfs.*`) - create a filesystem in a disk partition
- `sudo fdisk -l` - list / manipulate disk partitions (using MBR, hence only for <2TB disks)
- `gdisk` - GPT `fdisk` (supports disks >2TB)
- `sudo parted -l` - an alternative to `fdisk` / `gdisk`
- `rsync` - sync files to / from a remote host

##### SSH & HTTP
- `ssh -L 9999:remote-postgres:5432 bastion-host` - connect remotely to servers / create tunnels to access resources from a different network 
- `scp myapp.jar remote-server:/tmp/myapp-1.0.jar` - copy file(s) securely over SSH
- `wget http://example.com/backup.zip` - non-interactive download of files over HTTP(S) and FTP
- `curl -XGET http://example.com/api/v1/user/123` - transfer data over HTTP, FTP and other protocols 
- `ab -n 100 url` - Apache HTTP server benchmarking tool (WARNING! don't DDoS servers you don't own)

##### Networking
- `nmcli` - a cli tool for controlling NetworkManager
- `ping 8.8.8.8` - test connectity between host and provided IP 
- `traceroute 8.8.8.8` or `tracepath 8.8.8.8` - trace packets route to a host
- `dig example.com` or `nslookup example.com` - tools for "interrogating" DNS name servers 
- `ip address` (or now deprecated `ifconfig`) - show / manipulate network interfaces
- `ip route` (or now deprecated `route`) - show / manipulate the IP routing table
- `ip link set eth0 up/down` (`ifup`/`ifdown`) - enable / disable a network interface
- `ss -a` (or now deprecated `netstat`) - list open sockets
- `iw` (or now deprecated `ifconfig`) - show / manipulate wireless devices 
- `tcpdump -i eth0 port 80` - capture traffic off a network interface
- `tcpflow -c -i eth0 port 80` - capture and save traffic for analysis / debugging
- `nmap` - network exploration tool / port scanner
- `nc` (or more versatile `socat`) - listen on / connect to ports, forward packets etc. 

##### Shell Tools
- `env` - print all environment variables
- `history` - print the history of executed commands
- `which` - locate a command in *$PATH*
- `cat args.txt | xargs command` - turn each line of input into an argument for a command 
- `alias l='ls -l'` - create an alias for the command
- `sleep 30 > output.log 2>&1 &` - run a command in the background, redirecting its output to a file
- `jobs` - list processes started by the current shell (e.g. with `&` or by pressing *Ctrl+Z*)
- `fg` - run a previously suspended (or started in backgroud) process (spawed by the current shell) in the foreground
- `bg` - run a previously suspended process in the background 
- `nohup sleep 30 &` - allow the process to outlive the shell that it was started from (by ignoring HUP signal) 
- `time sleep 1` - measure time the command takes to execute 
- `watch` - run a command repeatedly, monitoring the output
- `cat my.txt | head -n 10` - limit output to first n lines
- `tail -n 10 -f output.log` - show last n lines and then keep reading from (following) the file
- `echo "hello" | tee trace.txt` - read from std in and write to std out cloning output to a file / another command 
- `cat output.log | grep keyword | wc -l` - count the number of (lines or words), here only those containing a keyword due to `grep`
- `vmstat 1 | awk '{print $1}'` - use AWK programming language (here used to extract 1st column of a tabular output)
- `cat file.json | jq .` - prettify / process JSON 

##### Special-Purpose Programs 
- `cal` - display a calendar
- `bc` - start a calculator language interpretor 
- `startx` - initialize an X session (very unlikely that you will ever need to use this command)
- `xrandr --output DP-2-2 --same-as eDP-1` - list / manage displays
- `update-grub` - generate grub config file (delegates to `grub-mkconfig`) 

## Recipies

### Creating a bootable USB stick from `.iso` file
- `umount /dev/sdb*`
- `sudo fdisk -l` and figure out how the USB drive is called (e.g. `/dev/sdb`)
- `sudo mkfs.vfat /dev/sdb -I`
- `dd if=~/Documents/ISOs/linuxmint-19-cinnamon-64bit-beta.iso of=/dev/sdb status=progress`

## Dotfiles & Configs

**NOTE**: Before Bash executes the shell-related dot files mentioned below, it also executes commands from `/etc/profile` or `/etc/bash.bashrc` (all depending on the type of the shell). You can find more info on this topic in `man bash`.

- `.bash_profile` - is executed (by Bash) for interactive login shells (in my case it simply sources `.bashrc`)
- `.bashrc` - is executed (by Bash) for interactive non-login shells (e.g. the ones started by Tmux, Guake or any other GUI Terminal)
- `.bash_aliases` - (at least in my case) is sourced directly by `.bashrc`, and contains various command aliases / functions
- `.profile` - is executed (by Sh, or other shell types if their config is missing) for interactive login shells
- `.inputrc` - configuration of GNU readline (I only use it to make Bash auto-completion ignore case) 
- `.selected_editor` - allows setting the default text editor, which, for example, is opened by `crontab -e` 
- `.gitconfig` - global configuration for Git (the version control tools)
- `.npmrc` - configuration for `npm`, NodeJS package manager
- `.tmux.conf` - configration for Tmux
- `.ssh/config` - configuration for SSH client (allows defining aliases for hosts,  keys to be used for each server etc.)
- `.gnupg/gpg.conf` - gpg/gpg2 configuration
- `.gnupg/gpg-agent.conf` - gpg agent configuration (if you are using gpg-agent instead of ssh-agent)

## Filesystem

- `~/.xsession-errors` - error log produced by X server (look here if you have troubles logging into the system)
- `~/.local/share/applications/` - stores user-specific desktop files (place your custom desktop files here) 

- `/usr/lib/jvm/` - directory where `apt` / `dpkg` will install Java
- `/var/log` - contains logs generated by the OS and other apps

## Appendix A: Repos / PPAs

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

## Appendix B: Software Checklist

- `apt install guake` - **Guake** - a dropdown terminal that can be shown / hidden with a shortcut
- `apt install tmux` - **Tmux**, a terminal multiplexer, e.g. allows splitting your terminal into panes
- `apt install vim-gnome` - **Vim**, a version of Vim that allows yanking/pasting to/from the clipboard (`"+y`/`"+p`) 
- `apt install direnv` - **Direnv**, unclutter your `.bashrc` by moving environment vars to individual `.envrc` files
- `apt install docker.io` - **Docker**, software for running containers ~*i.e. the lightweight VMs*~
- `apt install docker-compose` - **docker-compose**, bring up and tear down multi-container environments
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
- `snap install tldr` or `npm install -g tldr` - **TLDR**, a great collection of simplified man pages, first stop for help on any terminal command
- `snap install kubectl` - **kubectl**, a cli for working with k8s clusters

- `pip install --user s-tui` - **S-TUI**, a cli tool that graps cpu freq, utilization and temperature over time
- `pip install --upgrade --user awscli` - **AWS CLI**, a cli tool to work with Amazon Web Services 

To use some of the commands from this reference, you may also need to `apt install` the following packages:
- `finger`
- `htop` 
- `powertop` 
- `glances` 
- `jq` 
- `apache2-utils`

If you are developing on Java/Kotlin/Scala and hence require different versions of JDK, as well as Gradle, Maven or SBT, you may want to check out [SdkMan](https://sdkman.io/), which can help you out with this task. Alternativelly, you can try installing multiple JDKs via `apt` and switching between them via `update-java-alternatives` / `update-alternatives`, however I personally didn't get far with this approach because a) to install different versions of java I would end up using PPAs that I was not sure I could trust and b) installing a new JDK would sometimes break a previously installed one.

## Appendix C: Useful Links

After I had almost finished compiling my list of commands, I actually discovered at least a couple of Terminal references that were vastly overlapping with mine, so feel free to check them out as well (see the links below).

- [Bash shortcuts](https://github.com/fliptheweb/bash-shortcuts-cheat-sheet)
- [Vim shortcuts](https://vim.rtorr.com/)
- [Tmux shotcuts](https://gist.github.com/MohamedAlaa/2961058)
- [Linux Mint Community: Terminal Command Reference](https://community.linuxmint.com/tutorial/view/244)
- [Linux Commands - A practical reference](http://www.pixelbeat.org/cmdline.html)
- [100 Useful Unit Commands](http://oliverelliott.org/article/computing/ref_unix/)
- [Deprecated Linux networking commands](https://dougvitale.wordpress.com/2011/12/21/deprecated-linux-networking-commands-and-their-replacements/)
- [Advanced Bash-Scripting Guide](https://www.tldp.org/LDP/abs/html/abs-guide.html)
- [Guide to using YubiKey as a SmartCard for GPG and SSH](https://github.com/drduh/YubiKey-Guide#export-public-key)

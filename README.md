## RHCSA Prep

#### `P1` [qz](qz/p01.md) [ex](ex/p01.txt) [note](note/p01.md) Essential Tools
* `man` `info` `cat` `echo` `less` `more` `head` `tail`
* stdin stdout stderr, I/O redirection `>` `>>` `|` accepts stdout. `1>` `2>` `&>` `2>&1` `1>&2`
* `/dev/null`
* `grep` `egrep` `grep -E`
* `/etc/ssh/sshd_config`
* interactive shell & login shell
* `tar` `gzip`=`tar -z` `bzip2`=`tar -j` `star`
* editor `nano` `vim`
* symbolic/soft link `ln -s`. delete src file may cause broken link
* `usermod`
* `newgrp` - log in to a new group
* SetUID SetGID sticky-bit
* `umask` `/etc/bashrc` `/etc/profile`
* [`wheel` group](https://en.wikipedia.org/wiki/Wheel_(Unix_term))
* `man /usr/share/man` `info /usr/share/info` `/usr/share/doc` `rpm package` `apropos`(search the manual page names and descriptions) `mandb`
* `which` `whatis`(display manual page descriptions) `whereis`
* `find` `locate` `mlocate`-`updatedb` `apropos`-`mandb` `stat`-fileinfo
#### `P2` [qz](qz/p02.md) [ex](ex/p02.txt) [note](note/p02.md) Operate Running System
* `shutdown` `poweroff`-`init 0` `halt` `reboot`-`init 6`
* `systemctl -t help` list all types of unit configuration files
* `systemctl` `unit` `target` `service`
* `graphical.target` `multi-user.target` `isolate`
* `emergency.target` `rescue.target` 
* default target
* `systemctl list-units` `systemctl list-unit-files`
* Interrupt the boot process: edit grub config `systemd.unit=rescue.target`
* Interrupt the boot boot to gain access to a system: `rd.break` [link](https://www.certdepot.net/rhel7-interrupt-boot-gain-access-system/)
* ??? `man systemd.unit` `man systemd.service`
* `ps` `pgrep` `kill` `pkill` `top` `killall`
* `kill -l` print a list of signal names
* `nice` & `renice` `time`
* system load average `top` `w` `uptime`
* `cat /proc/cpuinfo`
* `/var/log/`
* `journalctl` log all events on system
* `systemd-analyze blame`
* `virsh`
* `port 22` `scp` `sftp`
#### `P3` [qz](qz/p03.md) [ex](ex/p03.txt) [note](note/p03.md) Configure Local Storage
* `cat /proc/partitions` [link](https://unix.stackexchange.com/questions/52215/determine-the-size-of-a-block-device)
* `fdisk` `gdisk` `blkid` `/etc/fstab`
* `mount` `umount`
* Regular disk: `/dev/xvdf` -> `fdisk/gdisk` -> `mkfs` -> `mount`
* LVM: `/dev/xvdf` -> `fdisk/gdisk` -> `pv` -> `vg` -> `lv` -> `mkfs` -> `mount`
* LVM clean: `lvremove` `vgremove` `pvremove`
* label xfs `xfs_admin -L myfsname /dev/xvdf1`
* label ext4 `tune2fs -L myfsname /dev/xvdf2`
* `mount -a` `umount -a` `df -h`
* `mkswap -L labelname`
* `swapon /dev/ship/myswap1` `swapoff /dev/xvdg1`
* `swapon -a` `swapoff -a`.
* Summary `swapon -s`. Equivalent to `cat /proc/swaps`
* `free -m`
#### `P4` [qz](qz/p04.md) [ex](ex/p04.txt) [note](note/p04.md) Create and Configure File System
* size of device `cat /proc/partitions` / `fdisk -l`
* `mkfs.vfat` `mkfs.ext4` ...
* `fsck.vfat` `fsck.ext4` ...
* `dumpe2fs` - dump `ext2/ext3/ext4` filesystem information
* `xfs_info` `xfs_admin` `xfs_repair`
* FS check & repair: `fsck.vfat` `fsck.ext4` `xfs_repair`
* **CIFS & NFS**
* `vgextend` `vgmove` `vgreduce`
* `lvextend` `xfs_growfs`-xfs `resize2fs`-ext
* set - GID
* `getfacl` `setfacl -m u` `setfacl -m g`
* ??? ACL mask
* `setfacl -d -m ...`
* subdirectories & files will inherit default ACL permission of parent directory.
#### `P5` [qz](qz/p05.md) [ex](ex/p05.txt) [note](note/p05.md) Deploy, Configure and Maintain Systems
* [Yum Commands](http://yum.baseurl.org/wiki/YumCommands)
* [update vs upgrade](https://askubuntu.com/questions/94102/what-is-the-difference-between-apt-get-update-and-upgrade)
* `ping` `ping6` `ifconfig` `ip` `netstat` `ss` `tracepath` `tracepath6` `traceroute` `traceroute6`
* **attached network device** `/sys/class/net`
* `nmcli` `/etc/sysconfig/network-scripts/`
  - `nmcli con show --active`
  - `nmcli connection delete myconn`
  - `nmcli connection add con-name "myconn-static" autoconnect no type ethernet ifname eth1 ip4 10.0.0.15 gw4 10.0.0.1`
* `hostname` `hostnamectl` `/etc/hosts`
* `/etc/resolv.conf` `dns` `nameserver`
* `Ctrl-D` = `<EOT>`
* limitation of `cron`: when the system reboots, all cron jobs will missing [link](https://serverfault.com/questions/52335/job-scheduling-using-crontab-what-will-happen-when-computer-is-shutdown-during)
* `anacron` is only for privileged users`
* `crontab -e` -> user
* `/etc/crontab` -> system
* `anacron` `anacron -n` `/var/spool/anacron`
* `systemctl` `enable` `is-enabled` `list-dependencies` `**.target.wants`
* **`kickstart`**
* gpgkey

#### `P6` [qz](qz/p06.md) [ex](ex/p06.txt) [note](note/p06.md) Manage Users and Groups
* `/etc/passwd` `/etc/group` `/etc/shadow`
* `/etc/passwd` -> `:/sbin/nologin`
* primary group & supplementary group
* `id` `groups`
* `useradd` `userdel -r` `usermod`
* `usermod`
  - lock/unlock user
  - change userid
  - `-c` change the GECOS field
  - `usermod -aG wheel mary`
  - `usermod -u 1111 mary`
  - `-s /sbin/nologin` change shell
* `/etc/login.defs` allows us to automatically sets specific parameters for our login password aging info
* password aging: command `chage`
* `groupadd`
* `getent group wheel`
* `newgrp` - log in to a new group
* `groupmod` `-h` `-n` `-g`
* `chown :grpname mydir`
* setUID setGID sticky bit
* realm
* ADLDAP

#### `P7` [qz](qz/p07.md) [ex](ex/p07.txt) [note](note/p07.md) Manage Security
* `firewalld` `firewall-cmd`
  - `--help`
  - `--get-zones`
  - `--get-default-zone`
  - `--list-all`
  - `--zone=home` `--zone=public`
  - `--permanent`
  - `--reload`
  - `--panic-on`
  - `--add-port` `--remove-port`
  - `--add-service` `--remove-service`
* `ssh-keygen` `ssh-copy-id`
  - file `.ssh/authorized_keys`
* passphrase `ssh-agent bash` `ssh-add`
* SELinux: enforcing permissive disabled
  - getenforce
  - setenforce 0
  - apropos selinux
  - /etc/selinux/config
* `semanage fcontext -l`
* `ps auxZ | grep httpd`
* `/.autorelabel`
* `restorecon -Rv /content`
* `getsebool`

---

#### Labs
* `P2` - [Kill or Adjust Process Priorities](lab/kill-adjust-process-priorities.pdf)
* `P3` - [Managing Logical Volumes on Red Hat Enterprise 7](lab/lvm-with-redhat7.pdf)
* `P3` - [Add and Remove Volumes, Partition Disks, and Work with LVM](lab/work-with-lvm.pdf)
* `P4` - [Mounting NFS Network File Systems](lab/mounting-nfs-network-fs.txt)
* `P4` - [Create and Mount SAMBA and CIFS Fileshares](lab/deploy-samba-server-rhcsa.pdf)
* `P4` - [Extending Existing Logical Volumes](lab/extending-lvm.pdf)
* `P5` - [Initializing Network Connectivity](lab/init-nw-conn.txt)
* `P5` - [Network Manager Sandbox](lab/nw-manager.txt)
* `P5` - [Update the Kernel Package to Ensure a Bootable System](lab/update-kernel-package.pdf)
* `P5` - [Installing and Updating Software](lab/install-update-software.txt)
* `P6` - [User Groups and Accounts Tasks](lab/user-groups-and-accounts.pdf)
* `P6` - [Using an Existing Authentication Service](lab/existing-auth-ad.pdf)
* `P6` - [Use Existing LDAP Credentials For Single Sign-On](lab/use-ldap-single-signon.pdf)
* `P7` - [Red Hat Security With FirewallD](lab/sec-with-firewalld.pdf)
* `P7` - [Regaining Access to a System](lab/regain-access-server.pdf)
* `P7` - [Configure SELinux and Add - Restore Security Contexts](lab/config-selinux.txt)
* `P7` - [Allowing Programs Through the Firewall](lab/allowing-prog-thru-firewall.txt)

---

#### Useful Link:

* [CertDepot - RHCSA](https://www.certdepot.net/rhel7/)

---

#### Trifle Stuff

* vnc port 5901
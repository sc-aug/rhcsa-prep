## RHCSA Prep

```
tmp: stick-bit & P4 last-3-lectures
```

* `P1` [qz](qz/p01.md) [ex](ex/p01.txt) [note](note/p01.md) Essential Tools
  - `man` `info` `cat` `echo` `less` `more` `head` `tail`
  - stdin stdout stderr, I/O redirection `>` `>>` `|` accepts stdout. `1>` `2>` `&>` `2>&1` `1>&2`
  - `/dev/null`
  - `grep` `egrep` `grep -E`
  - `/etc/ssh/sshd_config`
  - interactive shell & login shell
  - `tar` `gzip`=`tar -z` `bzip2`=`tar -j` `star`
  - editor `nano` `vim`
  - symbolic/soft link `ln -s`. delete src file may cause broken link
  - `usermod`
  - `newgrp` - log in to a new group
  - SetUID SetGID sticky-bit
  - `umask` `/etc/bashrc` `/etc/profile`
  - wheel group ??
  - `man /usr/share/man` `info /usr/share/info` `/usr/share/doc` `rpm package` `apropos` `mandb`
  - `which` `whatis` `whereis`
  - `find` `locate` `mlocate`-`updatedb` `apropos`-`mandb` `stat`-fileinfo
* `P2` [qz](qz/p02.md) [ex](ex/p02.txt) [note](note/p02.md) Operate Running System
  - `port 22` `scp` `sftp`
  - `systemctl` `unit` `target` `service`
  - `graphical.target` `multi-user.target` `isolate`
  - `default-target`
* `P3` [qz](qz/p03.md) [ex](ex/p03.txt) [note](note/p03.md) Configure Local Storage
  - `fdisk` `gdisk` `blkid` `/etc/fstab`
  - label xfs `xfs_admin -L myfsname /dev/xvdf1`
  - label ext4 `tune2fs -L myfsname /dev/xvdf2`
* `P4` [qz](qz/p04.md) [ex](ex/p04.txt) [note](note/p04.md) Create and Configure File System
  - `mkfs.vfat` `mkfs.ext4` ...
  - `fsck.vfat` `fsck.ext4` ...
  - `xfs_info` `xfs_admin` `xfs_repair`
  - `vgextend` `vgmove` `vgreduce`
  - `lvextend` `xfs_growfs`-xfs `resize2fs`-ext
  - set - GID
  - `getfacl`
* `P5` [qz](qz/p05.md) [ex](ex/p05.txt) [note](note/p05.md) Deploy, Configure and Maintain Systems
* `P6` [qz](qz/p06.md) [ex](ex/p06.txt) [note](note/p06.md) Manage Users and Groups
  - `/etc/passwd` `/etc/group` `/etc/shadow`
  - `id` `groups`
  - `useradd` `userdel` `usermod`
* `P7` [qz](qz/p07.md) [ex](ex/p07.txt) [note](note/p07.md) Manage Security

---

Labs
* `P4` - [Create and Mount SAMBA and CIFS Fileshares](lab/deploy-samba-server-rhcsa.pdf)
### List, Create and Delete Partitions on MBR and GPT Disks

#### MBR & GPT
* MBR - 32-bit based
  - partitions or device use MBR based partitions, *can only have 4 primary partitions*
  - each primary partition can only be only 2TB in size
* GPT - 64-bit based
  - runs on UEFI device
  - can have 128 primary partitions on a single device
  - each primary partition can be 8ZB in size

#### Partition attached device (MBR)
* attached device `xvdf` is shown in `/dev` directory
```
####### First partition
# fdisk /dev/xvdf
command: m ## show help menu
command: n ## new partition
...
Last sector, +sectors or +size{K,M,G} (2048-2097151, default 2097151): +500M
...
command: l
command: t  ## type of 83 'Linux' is default value, this step can be skipped
Selected partition 1
Hex code (type L to list all codes): 83
Changed type of partition 'Linux' to 'Linux'
command: w
# ls /dev | grep xvdf
#
#
####### Second partition
# fdisk /dev/xvdf
command: n ## new partition
...
Last sector, +sectors or +size{K,M,G} (1026048-2097151, default 2097151): +500M
Partition 2 of type Linux and of size 500 MiB is set
...
command: w
# ls /dev | grep xvdf
#
#
####### Create FS on this partition
# mkfs -t xfs /dev/xvdf1
#
#
####### Mount this device to system
# df -h  ## show all mounted device
# blkid  ## show available block storage devices that attached to the system
/dev/xvda2: UUID="668dbd02-c201-44bc-be76-f606fc9ab8db" TYPE="xfs" PARTUUID="9146b810-9a31-4c10-a206-01b0bbaca807"
/dev/xvda1: PARTUUID="c32f79de-9ad9-4d19-a8f2-f3a61628211b"
/dev/xvdf1: UUID="29a1c114-8c8d-4f6b-9b92-f2d857b13dfe" TYPE="xfs"
#
# cd /mnt && mkdir mymount
# # mount /dev/xvdf1 /mnt/mymount                             ### partition name may duplicate
# mount -U 29a1c114-8c8d-4f6b-9b92-f2d857b13dfe /mnt/mymount  ### better to use UUID.
# df -h
#
#
####### Umount device
# umount /mnt/mymount
#
#
####### Delete a partition
# fdisk /dev/xvdf
command: d ## delete partition
command: w
```

#### Partition attached device (GPT)
* not use `fdisk`, but `gdisk` or `parted`
```
# gdisk /dev/xvdf
Command (? for help): n
Partition number (1-128, default 1):
First sector (34-2097118, default = 2048) or {+-}size{KMGTP}:
Last sector (2048-2097118, default = 2097118) or {+-}size{KMGTP}: +500M
Current type is 'Linux filesystem'
Hex code or GUID (L to show codes, Enter = 8300): L
0700 Microsoft basic data  0c01 Microsoft reserved    2700 Windows RE
4200 Windows LDM data      4201 Windows LDM metadata  7501 IBM GPFS
7f00 ChromeOS kernel       7f01 ChromeOS root         7f02 ChromeOS reserved
8200 Linux swap            8300 Linux filesystem      8301 Linux reserved
8e00 Linux LVM             a500 FreeBSD disklabel     a501 FreeBSD boot
a502 FreeBSD swap          a503 FreeBSD UFS           a504 FreeBSD ZFS
a505 FreeBSD Vinum/RAID    a580 Midnight BSD data     a581 Midnight BSD boot
a582 Midnight BSD swap     a583 Midnight BSD UFS      a584 Midnight BSD ZFS
a585 Midnight BSD Vinum    a800 Apple UFS             a901 NetBSD swap
a902 NetBSD FFS            a903 NetBSD LFS            a904 NetBSD concatenated
a905 NetBSD encrypted      a906 NetBSD RAID           ab00 Apple boot
af00 Apple HFS/HFS+        af01 Apple RAID            af02 Apple RAID offline
af03 Apple label           af04 AppleTV recovery      af05 Apple Core Storage
be00 Solaris boot          bf00 Solaris root          bf01 Solaris /usr & Mac Z
bf02 Solaris swap          bf03 Solaris backup        bf04 Solaris /var
bf05 Solaris /home         bf06 Solaris alternate se  bf07 Solaris Reserved 1
bf08 Solaris Reserved 2    bf09 Solaris Reserved 3    bf0a Solaris Reserved 4
bf0b Solaris Reserved 5    c001 HP-UX data            c002 HP-UX service
ed00 Sony system partitio  ef00 EFI System            ef01 MBR partition scheme
ef02 BIOS boot partition   fb00 VMWare VMFS           fb01 VMWare reserved
fc00 VMWare kcore crash p  fd00 Linux RAID
Hex code or GUID (L to show codes, Enter = 8300): 8300
Changed type of partition to 'Linux filesystem'

Command (? for help): w

Final checks complete. About to write GPT data. THIS WILL OVERWRITE EXISTING
PARTITIONS!!

Do you want to proceed? (Y/N): y
OK; writing new GUID partition table (GPT) to /dev/xvdf.
The operation has completed successfully.
#
#
####### Check results
# ls /dev
#
# 
####### Create FS on partition
# mkfs -t xfs /dev/xvdf1
#
#
####### Mount this device to system
# blkid  ## show available block storage devices that attached to the system
/dev/xvda2: UUID="668dbd02-c201-44bc-be76-f606fc9ab8db" TYPE="xfs" PARTUUID="9146b810-9a31-4c10-a206-01b0bbaca807"
/dev/xvda1: PARTUUID="c32f79de-9ad9-4d19-a8f2-f3a61628211b"
/dev/xvdf1: UUID="bfd8c870-67c6-43ee-a0ee-510ff090e09d" TYPE="xfs" PARTLABEL="Linux filesystem" PARTUUID="fe5408dc-f4dd-44d9-839d-1e2e7b24fcdc"
#
# cd /mnt && mkdir mymount
# # mount /dev/xvdf1 /mnt/mymount                             ### partition name may duplicate
# mount -U bfd8c870-67c6-43ee-a0ee-510ff090e09d /mnt/mymount  ### better to use UUID.
# df -h
#
#
####### Umount device
# umount /mnt/mymount
#
#
####### Delete a partition
# gdisk /dev/xvdf
Found valid GPT with protective MBR; using GPT.

Command (? for help): d
Using 1

Command (? for help): w

Final checks complete. About to write GPT data. THIS WILL OVERWRITE EXISTING
PARTITIONS!!

Do you want to proceed? (Y/N): y
OK; writing new GUID partition table (GPT) to /dev/xvdf.
The operation has completed successfully.
```
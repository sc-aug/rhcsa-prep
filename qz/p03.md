CONFIGURE LOCAL STORAGE

1) What is the limit for primary partitions when using the MBR partition type?
* 4

2) You've just increased the size of the /dev/battlestar/galactica LVM volume. This volume is mounted in /mnt/mydir; which command would you issue for the operating system and file system to recognize the increase in size on the device?
* xfs_growfs /mnt/mydir

3) Given the volume group "battlestar" and a physical volume in the volume group /dev/xvdf1, which command would accurately create a 19 G logical volume out of /dev/xvdf1 with a volume name of "galactica"?
* lvcreate -n galactica -L 19G battlestar

4) Select the correct order of tasks for creating an LVM for the first time.
* Create the physical volume (pvcreate), create the volume group (vgcreate), create the logical volume (lvcreate)

5) Which file system is best used with LVM volumes?
* XFS

6) Which command displays information about a swap device?
* swapon -s

7) What are two ways to find information about swap devices enabled on the system?
* swapon -s, cat /proc/swaps

8) The file system needs to be created on the device after the logical volume is created (lvcreate).
* True

9) Which command is used to assign a swap signature to a device?
* mkswap

10) After making partition changes, the partition changes do not appear in the /proc/partitions. How can you force the kernel to reload the partition tables?
* issue the partprobe command

11) Swap space can be created out of GPT-, MBR- and LVM-based partitions/volumes.
* True

12) Which file on the operating system contains information about which partitions the OS is reading?
* /proc/partitions

13) What is the maximum disk size for a GPT-based partition?
* 8 ZiB

14) Which tool(s) would you use to mange GPT-based partitions?
* gdisk, parted

15) What command is used to inform the OS of partition table changes?
* partprobe

16) You are extending a logical volume. To do this, you have to add the /dev/xvdj device to the volume group "battlestar". Which command would you issue to accomplish this task?
* vgextend battlestar /dev/xvdj

17) For an MBR partition, what is the max disk size a partition can be?
* 2TiB

18) How many primary partitions can a GPT partition table have?
* 128

19) After adding 50 GB more of physical storage to your existing volume group, you need to extend your /dev/battlestar/galactica volume group to include an additional 20GB of storage. How might you accomplish this task?
* lvextend -L +20G /dev/battlestar/galactica

20) You're in the middle of the Red Hat certification test and forget which LVM commands you need to use to perform the specified tasks. What option(s) do you have?
* man lvm, info lvm

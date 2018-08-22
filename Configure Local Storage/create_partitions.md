Create the volume
```
[root@centos images]# dd if=/dev/zero of=/home/images/server1-disk1.img bs=1M count=1024
1024+0 records in
1024+0 records out
1073741824 bytes (1.1 GB) copied, 0.486349 s, 2.2 GB/s
```

Attach the volume to the virtual machine
```
[root@centos images]# virsh attach-disk server1 --source /var/lib/libvirt/images/server1-disk1.img --target vdb --persistent
Disk attached successfully
```

In the VM, use fdisk to create a new partition
```
[root@rhcsa1 dev]# fdisk /dev/vdb
Welcome to fdisk (util-linux 2.23.2).

Changes will remain in memory only, until you decide to write them.
Be careful before using the write command.

Device does not contain a recognized partition table
Building a new DOS disklabel with disk identifier 0x2374f605.

Command (m for help): n
Partition type:
   p   primary (0 primary, 0 extended, 4 free)
   e   extended
Select (default p): p
Partition number (1-4, default 1):
First sector (2048-2097151, default 2048):
Using default value 2048
Last sector, +sectors or +size{K,M,G} (2048-2097151, default 2097151):
Using default value 2097151
Partition 1 of type Linux and of size 1023 MiB is set

Command (m for help): m
Command action
   a   toggle a bootable flag
   b   edit bsd disklabel
   c   toggle the dos compatibility flag
   d   delete a partition
   g   create a new empty GPT partition table
   G   create an IRIX (SGI) partition table
   l   list known partition types
   m   print this menu
   n   add a new partition
   o   create a new empty DOS partition table
   p   print the partition table
   q   quit without saving changes
   s   create a new empty Sun disklabel
   t   change a partition's system id
   u   change display/entry units
   v   verify the partition table
   w   write table to disk and exit
   x   extra functionality (experts only)

Command (m for help): p

Disk /dev/vdb: 1073 MB, 1073741824 bytes, 2097152 sectors
Units = sectors of 1 * 512 = 512 bytes
Sector size (logical/physical): 512 bytes / 512 bytes
I/O size (minimum/optimal): 512 bytes / 512 bytes
Disk label type: dos
Disk identifier: 0x2374f605

   Device Boot      Start         End      Blocks   Id  System
/dev/vdb1            2048     2097151     1047552   83  Linux

Command (m for help): l

 0  Empty           24  NEC DOS         81  Minix / old Lin bf  Solaris
 1  FAT12           27  Hidden NTFS Win 82  Linux swap / So c1  DRDOS/sec (FAT-
 2  XENIX root      39  Plan 9          83  Linux           c4  DRDOS/sec (FAT-
 3  XENIX usr       3c  PartitionMagic  84  OS/2 hidden C:  c6  DRDOS/sec (FAT-
 4  FAT16 <32M      40  Venix 80286     85  Linux extended  c7  Syrinx
 5  Extended        41  PPC PReP Boot   86  NTFS volume set da  Non-FS data
 6  FAT16           42  SFS             87  NTFS volume set db  CP/M / CTOS / .
 7  HPFS/NTFS/exFAT 4d  QNX4.x          88  Linux plaintext de  Dell Utility
 8  AIX             4e  QNX4.x 2nd part 8e  Linux LVM       df  BootIt
 9  AIX bootable    4f  QNX4.x 3rd part 93  Amoeba          e1  DOS access
 a  OS/2 Boot Manag 50  OnTrack DM      94  Amoeba BBT      e3  DOS R/O
 b  W95 FAT32       51  OnTrack DM6 Aux 9f  BSD/OS          e4  SpeedStor
 c  W95 FAT32 (LBA) 52  CP/M            a0  IBM Thinkpad hi eb  BeOS fs
 e  W95 FAT16 (LBA) 53  OnTrack DM6 Aux a5  FreeBSD         ee  GPT
 f  W95 Ext'd (LBA) 54  OnTrackDM6      a6  OpenBSD         ef  EFI (FAT-12/16/
10  OPUS            55  EZ-Drive        a7  NeXTSTEP        f0  Linux/PA-RISC b
11  Hidden FAT12    56  Golden Bow      a8  Darwin UFS      f1  SpeedStor
12  Compaq diagnost 5c  Priam Edisk     a9  NetBSD          f4  SpeedStor
14  Hidden FAT16 <3 61  SpeedStor       ab  Darwin boot     f2  DOS secondary
16  Hidden FAT16    63  GNU HURD or Sys af  HFS / HFS+      fb  VMware VMFS
17  Hidden HPFS/NTF 64  Novell Netware  b7  BSDI fs         fc  VMware VMKCORE
18  AST SmartSleep  65  Novell Netware  b8  BSDI swap       fd  Linux raid auto
1b  Hidden W95 FAT3 70  DiskSecure Mult bb  Boot Wizard hid fe  LANstep
1c  Hidden W95 FAT3 75  PC/IX           be  Solaris boot    ff  BBT
1e  Hidden W95 FAT1 80  Old Minix

Command (m for help): t
Selected partition 1
Hex code (type L to list all codes): 83
Changed type of partition 'Linux' to 'Linux'

Command (m for help): m
Command action
   a   toggle a bootable flag
   b   edit bsd disklabel
   c   toggle the dos compatibility flag
   d   delete a partition
   g   create a new empty GPT partition table
   G   create an IRIX (SGI) partition table
   l   list known partition types
   m   print this menu
   n   add a new partition
   o   create a new empty DOS partition table
   p   print the partition table
   q   quit without saving changes
   s   create a new empty Sun disklabel
   t   change a partition's system id
   u   change display/entry units
   v   verify the partition table
   w   write table to disk and exit
   x   extra functionality (experts only)

Command (m for help): w
The partition table has been altered!

Calling ioctl() to re-read partition table.
Syncing disks.
[root@rhcsa1 dev]#
```

Vdb1 should now be available
```
[root@rhcsa1 dev]# ls
autofs         cpu              fuse          lp0              network_throughput  rtc       tty    tty17  tty26  tty35  tty44  tty53  tty62  uinput   vcs3   vcsa6        virtio-ports
block          cpu_dma_latency  hidraw0       lp1              null                rtc0      tty0   tty18  tty27  tty36  tty45  tty54  tty63  urandom  vcs4   vda          vport2p1
bsg            crash            hpet          lp2              nvram               sg0       tty1   tty19  tty28  tty37  tty46  tty55  tty7   usbmon0  vcs5   vda1         zero
btrfs-control  disk             hugepages     lp3              oldmem              shm       tty10  tty2   tty29  tty38  tty47  tty56  tty8   usbmon1  vcs6   vda2
bus            dm-0             hwrng         mapper           port                snapshot  tty11  tty20  tty3   tty39  tty48  tty57  tty9   usbmon2  vcsa   vdb
cdrom          dm-1             initctl       mcelog           ppp                 snd       tty12  tty21  tty30  tty4   tty49  tty58  ttyS0  usbmon3  vcsa1  vdb1
centos_rhcsa   dri              input         mem              ptmx                sr0       tty13  tty22  tty31  tty40  tty5   tty59  ttyS1  usbmon4  vcsa2  vfio
char           fb0              kmsg          mqueue           pts                 stderr    tty14  tty23  tty32  tty41  tty50  tty6   ttyS2  vcs      vcsa3  vga_arbiter
console        fd               log           net              random              stdin     tty15  tty24  tty33  tty42  tty51  tty60  ttyS3  vcs1     vcsa4  vhci
core           full             loop-control  network_latency  raw                 stdout    tty16  tty25  tty34  tty43  tty52  tty61  uhid   vcs2     vcsa5  vhost-net
[root@rhcsa1 dev]#
```

Format the Partition
```
[root@rhcsa1 dev]# mkfs -t xfs vdb1
meta-data=vdb1                   isize=512    agcount=4, agsize=65472 blks
         =                       sectsz=512   attr=2, projid32bit=1
         =                       crc=1        finobt=0, sparse=0
data     =                       bsize=4096   blocks=261888, imaxpct=25
         =                       sunit=0      swidth=0 blks
naming   =version 2              bsize=4096   ascii-ci=0 ftype=1
log      =internal log           bsize=4096   blocks=855, version=2
         =                       sectsz=512   sunit=0 blks, lazy-count=1
realtime =none                   extsz=4096   blocks=0, rtextents=0
[root@rhcsa1 dev]#
```

Verify that vdb1 is available for mounting
```
[root@rhcsa1 dev]# blkid
/dev/vda1: UUID="db513f3d-5880-4ddf-b364-35bbe9167e6e" TYPE="xfs"
/dev/vda2: UUID="uiE6Tf-ak2I-lAih-U4mO-dnbD-Vyzt-UfMpOy" TYPE="LVM2_member"
/dev/mapper/centos_rhcsa-root: UUID="0b8d7ed9-5de3-4dea-90e5-bef35fca402b" TYPE="xfs"
/dev/mapper/centos_rhcsa-swap: UUID="0dcbbe42-182f-4072-a527-fadc5f12ead5" TYPE="swap"
/dev/vdb1: UUID="be8264e8-99b9-4eaf-b0f3-2f9e09da6b55" TYPE="xfs"
```

Mount the Partition
```
[root@rhcsa1 mnt]# mount /dev/vdb1 /mnt/mymount/
[root@rhcsa1 mnt]# df
Filesystem                    1K-blocks    Used Available Use% Mounted on
/dev/mapper/centos_rhcsa-root   8374272 3933704   4440568  47% /
devtmpfs                         924236       0    924236   0% /dev
tmpfs                            941356       0    941356   0% /dev/shm
tmpfs                            941356    9456    931900   2% /run
tmpfs                            941356       0    941356   0% /sys/fs/cgroup
/dev/vda1                       1038336  173248    865088  17% /boot
tmpfs                            188272      12    188260   1% /run/user/42
tmpfs                            188272       0    188272   0% /run/user/0
/dev/vdb1                       1044132   32944   1011188   4% /mnt/mymount
[root@rhcsa1 mnt]#
```

Alternate way of mounting the partition is to use the UUID instead of the device name.
```
[root@rhcsa1 mnt]# mount -U be8264e8-99b9-4eaf-b0f3-2f9e09da6b55 /mnt/mymount
[root@rhcsa1 mnt]# df
Filesystem                    1K-blocks    Used Available Use% Mounted on
/dev/mapper/centos_rhcsa-root   8374272 3932680   4441592  47% /
devtmpfs                         924236       0    924236   0% /dev
tmpfs                            941356       0    941356   0% /dev/shm
tmpfs                            941356    9424    931932   2% /run
tmpfs                            941356       0    941356   0% /sys/fs/cgroup
/dev/vda1                       1038336  173248    865088  17% /boot
tmpfs                            188272      12    188260   1% /run/user/42
tmpfs                            188272       0    188272   0% /run/user/0
/dev/vdb1                       1044132   32944   1011188   4% /mnt/mymount
[root@rhcsa1 mnt]#
```

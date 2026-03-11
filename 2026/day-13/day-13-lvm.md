# Day 13 – Linux Volume Management (LVM)

## Task: Completed

Learn LVM to manage storage flexibly – create, extend, and mount volumes.

**Watch First:** [Linux LVM Tutorial](https://youtu.be/Evnf2AAt7FQ?si=ncnfQYySYtK_2K3c)

---

## Challenge Tasks

### Task 1: Check Current Storage

Run: `lsblk`, `pvs`, `vgs`, `lvs`, `df -h`

lsblk:

      ubuntu@ip-172-31-2-91:~$ lsblk

NAME MAJ:MIN RM SIZE RO TYPE MOUNTPOINTS
loop0 7:0 0 27.6M 1 loop /snap/amazon-ssm-agent/11797
loop1 7:1 0 73.9M 1 loop /snap/core22/2216
loop2 7:2 0 27.8M 1 loop /snap/amazon-ssm-agent/12322
loop3 7:3 0 50.8M 1 loop /snap/snapd/25202
loop4 7:4 0 74M 1 loop /snap/core22/2292
loop5 7:5 0 48.1M 1 loop /snap/snapd/25935
nvme0n1 259:0 0 15G 0 disk
├─nvme0n1p1 259:1 0 14G 0 part /
├─nvme0n1p14 259:2 0 4M 0 part
├─nvme0n1p15 259:3 0 106M 0 part /boot/efi
└─nvme0n1p16 259:4 0 913M 0 part /boot

### Task 2: Create Physical Volume

```bash
pvcreate /dev/sdb   # or your loop device
pvs
```

ubuntu@ip-172-31-2-91:~$ sudo lvm
lvm> pvcreate /dev/nvme1n1 /dev/nvme2n1
Physical volume "/dev/nvme1n1" successfully created.
Physical volume "/dev/nvme2n1" successfully created.
lvm> pvs
PV VG Fmt Attr PSize PFree
/dev/nvme1n1 lvm2 --- 10.00g 10.00g
/dev/nvme2n1 lvm2 --- 12.00g 12.00g

### Task 3: Create Volume Group

```bash
vgcreate devops-vg /dev/sdb
vgs
```

lvm> vgcreate tws_vg /dev/nvme1n1 /dev/nvme2n1
Volume group "tws_vg" successfully created
lvm> exit
Exiting.

### Task 4: Create Logical Volume

```bash
lvcreate -L 500M -n app-data devops-vg
lvs
```

lvm> lvcreate -L 10G -n tws-lv tws_vg
Logical volume "tws-lv" created.
lvm> exit
Exiting.

### Task 5: Format and Mount

```bash
mkfs.ext4 /dev/devops-vg/app-data
mkdir -p /mnt/app-data
mount /dev/devops-vg/app-data /mnt/app-data
df -h /mnt/app-data
```

sudo mkfs.ext4 /dev/tws_vg/tws-lv
mke2fs 1.47.0 (5-Feb-2023)
Creating filesystem with 2621440 4k blocks and 655360 inodes
Filesystem UUID: 8bbccc07-b6a1-4a1e-999f-3f9aa1e64ad7
Superblock backups stored on blocks:
32768, 98304, 163840, 229376, 294912, 819200, 884736, 1605632

Allocating group tables: done  
Writing inode tables: done  
Creating journal (16384 blocks): done
Writing superblocks and filesystem accounting information: done

ubuntu@ip-172-31-2-91:~$ sudo mount /dev/tws_vg/tws-lv /mnt/tws-lv-mount
ubuntu@ip-172-31-2-91:~$ df -h
Filesystem Size Used Avail Use% Mounted on
/dev/root 14G 6.9G 6.7G 51% /
tmpfs 456M 0 456M 0% /dev/shm
tmpfs 183M 924K 182M 1% /run
tmpfs 5.0M 0 5.0M 0% /run/lock
efivarfs 128K 3.8K 120K 4% /sys/firmware/efi/efivars
/dev/nvme0n1p16 881M 225M 595M 28% /boot
/dev/nvme0n1p15 105M 6.2M 99M 6% /boot/efi
tmpfs 92M 12K 92M 1% /run/user/1000
/dev/mapper/tws_vg-tws--lv 9.8G 24K 9.3G 1% /mnt/tws-lv-mount

### Task 6: Extend the Volume

```bash
lvextend -L +200M /dev/devops-vg/app-data
resize2fs /dev/devops-vg/app-data
df -h /mnt/app-data
```

lvm> lvextend -L +5G /dev/tws_vg/tws-lv
Size of logical volume tws_vg/tws-lv changed from 10.00 GiB (2560 extents) to 15.00 GiB (3840 extents).
Logical volume tws_vg/tws-lv successfully resized.
lvm> exit
Exiting.

ubuntu@ip-172-31-2-91:~$ df -h /mnt/tws-lv-mount
Filesystem Size Used Avail Use% Mounted on
/dev/mapper/tws_vg-tws--lv 9.8G 24K 9.3G 1% /mnt/tws-lv-mount

ubuntu@ip-172-31-2-91:~$ sudo resize2fs /dev/tws_vg/tws-lv
resize2fs 1.47.0 (5-Feb-2023)
Filesystem at /dev/tws_vg/tws-lv is mounted on /mnt/tws-lv-mount; on-line resizing required
old_desc_blocks = 2, new_desc_blocks = 2
The filesystem on /dev/tws_vg/tws-lv is now 3932160 (4k) blocks long.

ubuntu@ip-172-31-2-91:~$ df -h /mnt/tws-lv-mount
Filesystem Size Used Avail Use% Mounted on
/dev/mapper/tws_vg-tws--lv 15G 24K 14G 1% /mnt/tws-lv-mount

---

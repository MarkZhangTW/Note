Hyper-V
===

[ToC]

## Shrink VHDX for Linux VM

Shrinking the VHDX size may corrupt the GPT table, causing the VM to fail to boot.
As a result, VHDX shrinking should only be performed while the VM is powered on. Additionally, Hyper-V can only shrink a VHDX if there are no checkpoints.

```shell
sudo gdisk /dev/sda
```
:::spoiler
GPT fdisk (gdisk) version 1.0.7

Warning! Disk size is smaller than the main header indicates! Loading
secondary header from the last sector of the disk! You should use 'v' to
verify disk integrity, and perhaps options on the experts' menu to repair
the disk.
Caution: invalid backup GPT header, but valid main header; regenerating
backup header from main header.

Warning! One or more CRCs don't match. You should repair the disk!
Main header: OK
Backup header: ERROR
Main partition table: OK
Backup partition table: ERROR

Partition table scan:
  MBR: protective
  BSD: not present
  APM: not present
  GPT: damaged

****************************************************************************
Caution: Found protective or hybrid MBR and corrupt GPT. Using GPT, but disk
verification and recovery are STRONGLY recommended.
****************************************************************************

Command (? for help): **v**

Caution: The CRC for the backup partition table is invalid. This table may
be corrupt. This program will automatically create a new backup partition
table when you save your partitions.

Problem: The secondary header's self-pointer indicates that it doesn't reside
at the end of the disk. If you've added a disk to a RAID array, use the 'e'
option on the experts' menu to adjust the secondary header's and partition
table's locations.

Problem: Disk is too small to hold all the data!
(Disk size is 25165824 sectors, needs to be 33554432 sectors.)
The 'e' option on the experts' menu may fix this problem.

Problem: GPT claims the disk is larger than it is! (Claimed last usable
sector is 33554398, but backup header is at
33554431 and disk size is 25165824 sectors.
The 'e' option on the experts' menu will probably fix this problem

Partition(s) in the protective MBR are too big for the disk! Creating a
fresh protective or hybrid MBR is recommended.

Identified 5 problems!

Command (? for help): **x**

Expert command (? for help): **e**
Relocating backup data structures to the end of the disk

Expert command (? for help): **w**

Final checks complete. About to write GPT data. THIS WILL OVERWRITE EXISTING
PARTITIONS!!

Do you want to proceed? (Y/N): **^**
OK; writing new GUID partition table (GPT) to /dev/sda.
Warning: The kernel is still using the old partition table.
The new table will be used at the next reboot or after you
run partprobe(8) or kpartx(8)
The operation has completed successfully.
:::
```shell
sudo partprobe
sudo gdisk /dev/sda
```
:::success
GPT fdisk (gdisk) version 1.0.7

Partition table scan:
  MBR: protective
  BSD: not present
  APM: not present
  GPT: present

Found valid GPT with protective MBR; using GPT.
:::

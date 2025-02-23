## 1. Determine the Device and Partition:

First, you need to identify the device and partition you want to resize. 
<br>Use the lsblk or fdisk -l commands to list your block devices and their partitions.
<br>Example lsblk output:
<br>NAME   MAJ:MIN RM   SIZE RO TYPE MOUNTPOINT
<br>sda      8:0    0   20G  0 disk
<br>├─sda1   8:1    0   10G  0 part /
<br>├─sda2   8:2    0   1G   0 part [SWAP]
<br>└─sda3   8:3    0    9G  0 part /data
In this example, if you wanted to resize /data (sda3), you'd be working with /dev/sda3.

## 2. Unmount the Partition (If Necessary):

If the partition is mounted, you must unmount it before resizing.
<br>Example: sudo umount /dev/sda3
If you are resizing the root partition, you will have to do it from a live usb or another recovery environment.

## 3. Extend the Partition (Using fdisk, parted, or growpart):

Before resizing the file system, you must extend the partition itself.
<br>This is done using tools like fdisk, parted, or growpart. growpart is recommended for cloud environments.
<br>Using growpart (Recommended for Cloud):
<br>sudo growpart /dev/sda 3 (where /dev/sda is the device and 3 is the partition number).
<br>This command tells growpart to extend the third partition of /dev/sda to fill the available space.

Using parted:
sudo parted /dev/sda
resizepart
3 (partition number)
100% (or the desired end size)
quit
Using fdisk (More complex):
sudo fdisk /dev/sda
p (print partition table)
d (delete the partition)
3 (partition number)
n (new partition)
p (primary partition)
3 (partition number)
Press Enter for the first sector (default).
Press Enter for the last sector (default, to use all available space).
w (write changes and exit).
After using fdisk, you may need to use partprobe /dev/sda to have the kernel recognize the new partition size.

## 4. Resize the File System (Using resize2fs):

Now that the partition is extended, you can resize the file system to use the new space.
sudo resize2fs /dev/sda3
This command will automatically resize the file system to fill the partition.
If you want to resize to a specific size, you can specify it: sudo resize2fs /dev/sda3 15G
If you have issues, running a filesystem check before the resize can help. sudo e2fsck -f /dev/sda3

## 5. Mount the Partition (If Necessary):

If you unmounted the partition, remount it.
sudo mount /dev/sda3 /data

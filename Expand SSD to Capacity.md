# Resizing a Filesystem with resize2fs

`resize2fs` is a command-line utility for resizing ext2, ext3, and ext4 file systems. This guide explains how to expand a partition to its full capacity.

**Important:** Always back up your data before performing any partition resizing. Data loss can occur if something goes wrong.

## Steps

1.  **Determine the Device and Partition:**

    * Use `lsblk` or `fdisk -l` to identify the device and partition you want to resize.
        * Example `lsblk` output:
            ```
            NAME   MAJ:MIN RM   SIZE RO TYPE MOUNTPOINT
            sda      8:0    0   20G  0 disk
            ├─sda1   8:1    0   10G  0 part /
            ├─sda2   8:2    0   1G   0 part [SWAP]
            └─sda3   8:3    0    9G  0 part /data
            ```
        * In this example, `/dev/sda3` is the partition for `/data`.

2.  **Unmount the Partition (If Necessary):**

    * If the partition is mounted, unmount it:
        ```bash
        sudo umount /dev/sda3
        ```
    * For the root partition, you'll need to use a live USB or recovery environment.

3.  **Extend the Partition (Using `growpart`, `parted`, or `fdisk`):**

    * **Using `growpart` (Recommended for Cloud):**
        ```bash
        sudo growpart /dev/sda 3
        ```
        * This extends `/dev/sda3` to the available space.
    * **Using `parted`:**
        ```bash
        sudo parted /dev/sda
        resizepart
        3
        100%
        quit
        ```
    * **Using `fdisk` (More Complex):**
        ```bash
        sudo fdisk /dev/sda
        p  # Print partition table
        d  # Delete partition
        3  # Partition number
        n  # New partition
        p  # Primary partition
        3  # Partition number
        # Press Enter for first sector (default)
        # Press Enter for last sector (default)
        w  # Write changes and exit
        ```
        * After `fdisk`, run `sudo partprobe /dev/sda` to update the kernel.

4.  **Resize the Filesystem (Using `resize2fs`):**

    * Resize the filesystem to use the new space:
        ```bash
        sudo resize2fs /dev/sda3
        ```
    * To resize to a specific size:
        ```bash
        sudo resize2fs /dev/sda3 15G
        ```
    * Run a filesystem check before resizing if you have issues:
        ```bash
        sudo e2fsck -f /dev/sda3
        ```

5.  **Mount the Partition (If Necessary):**

    * Remount the partition:
        ```bash
        sudo mount /dev/sda3 /data
        ```

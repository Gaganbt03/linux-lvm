# LVM Practice Using Loop Devices (Linux)
This file contains all the commands used to practice LVM (Logical Volume Manager) using loop devices instead of real disks.

---
```bash
# 1 Creating Volumes
      sudo dd if=/dev/zero of=/tmp/disk2.img bs=1G count=3 status=progress
      LOOP0=$(sudo losetup -fP --show /tmp/disk2.img)
      echo "disk2.img attached to $LOOP0"
      
      sudo dd if=/dev/zero of=/tmp/disk3.img bs=1G count=2 status=progress
      LOOP1=$(sudo losetup -fP --show /tmp/disk3.img)
      echo "disk3.img attached to $LOOP1"
      
      sudo dd if=/dev/zero of=/tmp/disk4.img bs=1G count=3 status=progress
      LOOP2=$(sudo losetup -fP --show /tmp/disk4.img)
      echo "disk4.img attached to $LOOP2"
      
# 2 Create Physical Volumes
    sudo pvcreate $LOOP0 $LOOP1 $LOOP2
    # verify Physical Volumes
    pvdisplay
# 3 Create Volume Group
    sudo vgcreate devops_vg $LOOP0 $LOOP1
    # Verify Volume Group
    vgdisplay
# 4 Create Logical Volume
    sudo lvcreate -L 3G -n devops_lv devops_vg
    # Verify Logical Volume
    lvdisplay
# 5 Format the Logical Volume
    sudo mkfs.ext4 /dev/devops_vg/devops_lv
# 6 Create Mount Directory
    sudo mkdir /mnt/devops_dir
# 7 Mount the Logical Volume
    sudo mount /dev/devops_vg/devops_lv /mnt/devops_dir
    # Verify Mount
    df -h
# 8 Final Verification
    lsblk
```
---

## Commands Used and Their Explanation
1. dd: Used to create disk image files which act like virtual hard disks.
2. losetup: Used to attach disk image files to loop devices.
3. pvcreate: Used to create Physical Volumes (PV) from loop devices.
4. pvdisplay: Used to display details of Physical Volumes.
5. vgcreate: Used to create a Volume Group (VG) using Physical Volumes.
6. vgdisplay: Used to display information about the Volume Group.
7. lvcreate: Used to create a Logical Volume (LV) from a Volume Group.
8. lvdisplay: Used to display details of the Logical Volume.
9. mkfs.ext4: Used to create an ext4 file system on the Logical Volume.
10. mkdir: Used to create a directory for mounting the Logical Volume.
11. mount: Used to mount the Logical Volume to a directory.
12. df -h: Used to check disk space usage and mounted file systems.
13. lsblk: Used to view block devices and verify the LVM structure.

---



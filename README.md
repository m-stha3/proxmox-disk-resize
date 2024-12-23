# proxmox-disk-resize
A comprehensive guide for increasing disk size in Proxmox virtual machines. This repository provides step-by-step instructions for resizing disk space in Proxmox, including resizing LVM partitions and filesystems within the VM. Ideal for system administrators and users looking to manage VM storage effectively.

# Increasing Disk Size in Proxmox VM

This guide provides a comprehensive step-by-step process for increasing the disk size of a VM in Proxmox and resizing the filesystem within the VM.

## Prerequisites

- Access to the Proxmox web interface.
- SSH or console access to the VM.
- Ensure that you have backups of any important data before proceeding.

## Step 1: Resize the Disk in Proxmox

1. **Access Proxmox Web Interface**: 
   - Open your web browser and log into your Proxmox web interface.
   
2. **Select the VM**: 
   - Click on the VM for which you want to increase the disk size.

3. **Go to Hardware**: 
   - In the VM’s menu, select the "Hardware" tab.

4. **Select the Disk**: 
   - Click on the disk you want to resize (typically labeled as `scsi0`, `virtio0`, etc.).

5. **Resize Disk**:
   - Click the "Resize" button.
   - Enter the new size in GB (e.g., if you want to increase it to 100 GB, enter `100`).
   - Click "Resize Disk" to apply the changes.

## Step 2: Resize the LVM Partition Inside the VM

1. **Log into the VM**: 
   - Use SSH or console access to log into the VM.

2. **Check the Current Logical Volumes**: 
   - Run the following command:
     ```bash
     sudo lvdisplay
     ```
   - This will show you the logical volumes and their sizes.

3. **Resize the Physical Volume**:
   - Resize the physical volume to recognize the new space:
     ```bash
     sudo pvresize /dev/sda3
     ```

4. **Resize the Logical Volume**:
   - To extend the logical volume to use all available space:
     ```bash
     sudo lvextend -l +100%FREE /dev/ubuntu-vg/ubuntu-lv
     ```
   - Alternatively, specify a size, e.g., to increase it by 10 GB:
     ```bash
     sudo lvextend -L +10G /dev/ubuntu-vg/ubuntu-lv
     ```

5. **Resize the Filesystem**:
   - For `ext4` filesystems:
     ```bash
     sudo resize2fs /dev/ubuntu-vg/ubuntu-lv
     ```
   - For `xfs` filesystems, if applicable:
     ```bash
     sudo xfs_growfs /
     ```

## Step 3: Verify the Changes

1. **Check the Available Space**:
   - Run the following command to check the updated size of the filesystem:
     ```bash
     df -h
     ```

## Conclusion

By following these steps, you will have successfully increased the disk size of your VM and resized the filesystem within it. Always ensure to back up important data before making significant changes to disk sizes. If you encounter any issues, consult the Proxmox documentation or seek assistance.

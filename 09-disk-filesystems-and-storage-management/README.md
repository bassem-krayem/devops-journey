# Disk, Filesystems, and Storage Management

### 1. Listing Block Devices (`lsblk`)

- `lsblk` -> List all block devices and partitions in a tree view.
- `lsblk -d` -> Show physical disks only (omit partitions).
- `lsblk -f` -> Display filesystem type (ext4, xfs, etc.) and UUIDs.
- `lsblk -b` -> Output size in exact bytes instead of human-readable units.

---

### 2. Mounting & Persistent Mounts (`fstab`)

```bash
# Basic Loop Mount
sudo mkdir -p /mnt/diskname
sudo mount -o loop image.img /mnt/diskname
sudo umount /mnt/diskname

# Permanent Mounts (/etc/fstab)
sudo cp /etc/fstab /etc/fstab.bak      # Always backup before editing
sudo nano /etc/fstab                   # Edit configuration
sudo mount -a                          # Test entries without rebooting (No output = Success)

```

---

### 3. Space & Inode Analysis (`df`, `du`, `find`)

```bash
# Find Large Files & Directories
find . -type f -size +100M             # Find files larger than 100MB
sudo du -h --max-depth=1 /var | sort -hr # Find top disk space consumers in /var

# Inode Usage (Index Cards for metadata)
df -i                                  # Check remaining Inode capacity per filesystem
ls -i sample_file.txt                  # Show specific Inode number for a file

```

> **Key Concept:** Storage has two limits—**Bytes** (disk size) and **Inodes** (file count). Creating 1,000,000 empty files will run you out of Inodes before running out of disk bytes.

---

### 4. Logical Volume Management (LVM)

LVM decouples physical storage from the operating system, allowing dynamic disk resizing without downtime.

#### The LVM Hierarchy

1. **Physical Volume (PV):** Raw disks/partitions prepped for LVM (`pvcreate`).
2. **Volume Group (VG):** Combining PVs into one large storage pool (`vgcreate`).
3. **Logical Volume (LV):** Virtual partitions carved out of the VG (`lvcreate`).

#### Step-by-Step LVM Workflow

```bash
# Step 1: Create & View Physical Volumes (PV)
sudo pvcreate /dev/loop20 /dev/loop21
sudo pvs                               # Quick summary of PVs

# Step 2: Combine PVs into a Volume Group (VG)
sudo vgcreate vg_data /dev/loop20 /dev/loop21
sudo vgs                               # Quick summary of VGs
sudo vgdisplay vg_data                 # Detailed view of storage pool

# Step 3: Carve out a Logical Volume (LV)
sudo lvcreate -L 1G -n lv_logs vg_data # Create a 1GB LV named lv_logs inside vg_data
sudo lvs                               # Quick summary of LVs

# Step 4: Format with Filesystem & Mount
sudo mkfs.ext4 /dev/vg_data/lv_logs
sudo blkid /dev/vg_data/lv_logs        # View filesystem UUID
sudo mkdir -p /mnt/logs
sudo mount /dev/vg_data/lv_logs /mnt/logs

# Step 5: Expand LV Dynamically (On-The-Fly)
sudo lvextend -L +500M /dev/vg_data/lv_logs # Extend LV size by 500MB
sudo resize2fs /dev/vg_data/lv_logs         # Expand ext4 filesystem to use new space
df -h /mnt/logs                             # Verify expanded capacity

```

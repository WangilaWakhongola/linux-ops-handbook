# 8. Disks & Storage

## Checking disk usage

```bash
df -h                    # disk space per mounted filesystem
df -i                       # inode usage (can run out even with free space)
du -sh /var/log              # size of a specific directory
du -sh /* 2>/dev/null            # size of every top-level directory
ncdu /                              # interactive disk usage explorer (if installed)
```

## Listing disks and partitions

```bash
lsblk                  # list block devices in a tree view
fdisk -l                  # list partitions (needs sudo)
blkid                       # show filesystem types and UUIDs
mount | column -t              # show currently mounted filesystems
```

## Mounting and unmounting

```bash
sudo mount /dev/sdb1 /mnt/data       # mount a device
sudo umount /mnt/data                   # unmount
cat /etc/fstab                             # persistent mounts (loaded at boot)
```

## Creating filesystems (be careful — destructive)

```bash
sudo mkfs.ext4 /dev/sdb1        # format a partition as ext4
sudo mkswap /dev/sdb2              # create swap space
sudo swapon /dev/sdb2                # enable swap
swapon --show                          # show active swap
```

## Finding what's eating disk space

```bash
du -ah / 2>/dev/null | sort -rh | head -20    # top 20 largest files/dirs
find / -size +500M -exec ls -lh {} \;             # files larger than 500MB
```

## Common gotchas

- "No space left on device" can mean inodes are exhausted, not actual disk space — check `df -i` too.
- `du` and `df` can disagree if a file is deleted while still held open by a running process — the space isn't freed until the process closes it.
- Always double, triple-check the device name (`/dev/sdX`) before running `mkfs` — wrong target wipes the wrong disk.

# LVM Storage Role

This Ansible role configures LVM volumes with RAID support for hypervisor storage pools.

## Description

This role sets up three storage pools for VM storage:

1. **Fast Pool**: RAID 1 array across multiple drives (sdb-sde) with LVM
2. **Slow Pool**: Single drive (sdf) with LVM
3. **Mid Pool**: Simple directory (no LVM)

## Requirements

- Debian/Ubuntu-based system (for mdadm and update-initramfs)
- Physical disks available at specified paths
- Root/sudo access

## Role Variables

### Fast Pool (RAID 1)
- `lvm_fast_pool_devices`: List of devices for RAID 1 array (default: /dev/sdb, /dev/sdc, /dev/sdd, /dev/sde)
- `lvm_fast_pool_vg_name`: Volume group name (default: "fast-pool")
- `lvm_fast_pool_lv_name`: Logical volume name (default: "fast-pool-lv")
- `lvm_fast_pool_mount_point`: Mount point path (default: "/var/lib/vms/fast-pool")
- `lvm_fast_pool_filesystem`: Filesystem type (default: "ext4")
- `lvm_fast_pool_raid_level`: RAID level (default: "raid1")
- `lvm_fast_pool_lv_size`: Logical volume size (default: "100%FREE")

### Slow Pool
- `lvm_slow_pool_devices`: List of devices (default: /dev/sdf)
- `lvm_slow_pool_vg_name`: Volume group name (default: "slow-pool")
- `lvm_slow_pool_lv_name`: Logical volume name (default: "slow-pool-lv")
- `lvm_slow_pool_mount_point`: Mount point path (default: "/var/lib/vms/slow-pool")
- `lvm_slow_pool_filesystem`: Filesystem type (default: "ext4")
- `lvm_slow_pool_lv_size`: Logical volume size (default: "100%FREE")

### Mid Pool
- `lvm_mid_pool_directory`: Directory path (default: "/var/lib/vms/mid-pool")

### General
- `lvm_mount_options`: Mount options (default: "defaults,noatime")
- `lvm_wipe_signatures`: Whether to wipe existing signatures (default: yes)

## Example Playbook

```yaml
- name: Configure storage
  hosts: hypervisor
  become: true
  roles:
    - role: lvm
      tags: ["storage", "lvm"]
```

## Tags

- `system`: General system configuration
- `storage`: Storage-related tasks
- `lvm`: LVM-specific tasks

## Warning

⚠️ This role will **wipe existing data** on the specified devices when `lvm_wipe_signatures` is set to `yes`. Make sure you have backups before running this role.

## Dependencies

None

## License

See repository LICENSE file

## Author Information

Created for ansible-hypervisor project


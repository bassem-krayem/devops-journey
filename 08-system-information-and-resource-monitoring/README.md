# System Information & Resource Monitoring

### 1. System & OS Info

- `uname -a` -> Kernel version, architecture, and system details.
- `cat /etc/os-release` -> Linux distribution name and version details.
- `uptime` -> System run time, logged-in users, and load averages.
- `nproc` -> Number of available processing units (CPU cores).

---

### 2. Memory & Swap (`free`, `swapon`)

- `free` -> Display total, used, and available RAM in kilobytes.
- `free -h` -> Display RAM usage in human-readable format (MB/GB).
- `swapon --show` -> List active swap partitions/files and usage.

---

### 3. Disk Usage (`df`, `du`)

- `df -h` -> Display filesystem disk space usage (human-readable).
- `df -h /` -> Show available space specifically on the root `/` partition.
- `sudo du -sh /var/log` -> Summary (`-s`) of total directory size in human-readable (`-h`) format.
- `du -ah` -> List sizes of all (`-a`) files and directories recursively.

---

### 4. Real-Time Monitoring (`watch`)

- `watch -d <command>` -> Periodically rerun command (default: every 2s) and highlight (`-d`) changes between updates.
  - _Example:_ `watch -d free -h`

# Users, Groups, and Permissions

### Key Files & Commands

- **`/etc/passwd`**: System file storing user account details.
- **`id`**: Displays current user's UID, GID, and group memberships.

---

### Ownership (`chown` / `chgrp`)

View ownership with `ls -l` (column 3 = User, column 4 = Group).

```bash
sudo chown user file.txt         # Change user owner
sudo chgrp group file.txt        # Change group owner
sudo chown user:group file.txt   # Change both at once

```

---

### Permissions Structure

Run `ls -l` to view standard permission bits (10 characters):
`drwxr-xr--` $\rightarrow$ `[Type][Owner][Group][Others]`

- **Type:** `-` = File, `d` = Directory
- **Bits:** `r` = Read (4), `w` = Write (2), `x` = Execute (1)

---

### Modifying Permissions (`chmod`)

**1. Symbolic Mode** (`u`=user, `g`=group, `o`=others, `a`=all)

```bash
chmod u+x file.txt    # Add execute for owner
chmod g-w file.txt    # Remove write for group
chmod +x file.txt     # Add execute for everyone (a+x)

```

**2. Absolute Mode (Octal Numbers)**
Sum values for each section (`r=4`, `w=2`, `x=1`):

| Code    | Permissions | Description            |
| ------- | ----------- | ---------------------- |
| **`7`** | `rwx`       | Read + Write + Execute |
| **`6`** | `rw-`       | Read + Write           |
| **`4`** | `r--`       | Read Only              |
| **`0`** | `---`       | No Permissions         |

```bash
chmod 644 file.txt    # rw-r--r-- (Owner: RW, Group: R, Others: R)
chmod 755 script.sh   # rwxr-xr-x (Owner: RWX, Group: RX, Others: RX)
chmod 600 key.pem     # rw------- (Owner: RW, Group: None, Others: None)

```

# Superuser Access & `umask`

### Superuser (`sudo`)

- **`sudo <cmd>`**: Executes a single command with root privileges.
- **`sudo -i`**: Opens an interactive root shell (loads root's environment and sets working directory to `/root`).

---

### Default File Creation Mask (`umask`)

`umask` defines default permissions for newly created files and directories by **subtracting (masking)** bits from base values.

- **Base values:** Files = `666` (`rw-rw-rw-`) | Directories = `777` (`rwxrwxrwx`)

```bash
# View current mask
umask

# Set new mask
umask 002

```

Calculation Example (umask 002):

- Files: 666 - 002 = 664 (rw-rw-r--) Owner/Group: RW, Others: R
- Directories: 777 - 002 = 775 (rwxrwxr-x) Owner/Group: RWX, Others: RX

Note: To make umask permanent, add umask 002 to ~/.bashrc or /etc/profile.

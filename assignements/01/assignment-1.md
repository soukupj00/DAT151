# DAT151: Assignment 1 Report

**Group Members:** [Insert Names Here] 

**Date:** [Insert Date]

---

## Part 1 - The Filesystem

### Task 1: Filesystem basics

**1. Key differences between Linux/UNIX and Windows filesystems:**

**Hierarchy:** Linux uses a single unified tree structure starting at the root (`/`), whereas Windows uses drive letters (C:, D:) for different partitions.
 
**Case Sensitivity:** Linux filenames are case-sensitive (`File.txt` and `file.txt` are different), while Windows is generally case-insensitive.
 
**File Extensions:** Linux identifies file types by metadata/content rather than strictly relying on extensions like `.exe` or `.txt`.

**2. Mount Points and Partitioning:**

**Definition:** A "mount point" is a directory in the existing filesystem hierarchy where a new filesystem (like a partition) is attached to make it accessible.

**Command Execution:**

```bash
# Create the partition (based on free space starting at 481042432s)
sudo parted -s /dev/sda mkpart "dat151_partition" xfs 481042432s 100%

# Format the new partition (assuming it is /dev/sda4)
sudo mkfs -t xfs /dev/sda4

# Create the folder and mount
sudo mkdir /opt/new_folder
sudo mount /dev/sda4 /opt/new_folder

```

**Outcome Explanation:** The directory `/opt/new_folder` now acts as the gateway to the 10GiB physical partition. Files stored here do not consume space on the root partition.

**[Insert Screenshot of `df -h` and `lsblk` here]**

**3. The Seven Types of Files:**
The seven types are: Regular files, Directories, Character devices, Block devices, Local domain sockets, Named pipes (FIFO), and Symbolic links.

**Command Output:**

```bash
ls -ld /etc/passwd /etc /dev/sda4 /dev/tty

```

**Categorization:** * `/etc/passwd`: Regular file (`-`).
 
`/etc`: Directory (`d`).

`/dev/sda4`: Block device (`b`).

`/dev/tty`: Character device (`c`).

**4. Hard Links vs. Symbolic Links:**

**Execution:**

```bash
echo "test" > original.txt
ln original.txt hardlink
ln -s original.txt softlink
rm original.txt

```

**a) Hard Link result:** Still works. It points to the same inode (data) as the original file.

**b) Soft Link result:** Fails/Broken. It points to a filename that no longer exists.

---

## Task 2: File attributes and permissions

**1. The 16 bits:**
The 16 bits include 9 for permissions (rwx for user, group, others), 3 for special modes (SUID, SGID, Sticky bit), and 4 for file type.

**2. Octal Representation:**

`110 100 101` in octal is **645**.

Octal **745** corresponds to: `rwxr--r-x` (User: rwx, Group: r, Others: rx).

**3. Chmod Commands:**

**a) Mnemonic:** `chmod u=rw,g=rx,o=rx file1.txt`.

**b) Octal:** `chmod 655 file2.txt`.

**c) Preference:** Octal is often preferred for its brevity and ability to set all permissions absolutely in a single, consistent 3-digit command.

---

## Part 2 - Software installation and management

### Task 3: Lower level package management

1. Definition: A system to automate software installation and tracking. AlmaLinux uses the **.rpm** format.
**2. RPM Operations:**

**a) Count:** `rpm -qa | wc -l`.

**b) Download:** `wget [URL_TO_OPENVPN_RPM]`.

**c) Result of `rpm -ivh`:** Fails due to missing dependencies. `rpm` cannot resolve or download required libraries automatically.

### Task 4: High-level package management

1. DNF System: DNF (Dandified YUM) locates software in remote repositories and automatically handles dependency resolution.
2. Repository: A storage location for software packages.

Command: `sudo dnf install epel-release`.
3. OpenVPN via DNF: Unlike Task 3, DNF successfully installs OpenVPN by automatically fetching all dependencies.

Remove command: `sudo dnf remove openvpn`.
4. Update: `sudo dnf update`.

---

## Part 3 - Systemd

### Task 1: Systemd Targets and Units

**1. ctrl-alt-del.target:**

**a) Current:** Points to `reboot.target`, causing an immediate restart.

**b) Modification:**

```bash
sudo ln -sf /usr/lib/systemd/system/halt.target /etc/systemd/system/ctrl-alt-del.target
sudo systemctl daemon-reload
[cite_start]``` [cite: 94, 95]
```

**2. Default Target:**

**Current Value:** [Insert recorded value, e.g., graphical.target].

**Change:** `sudo systemctl set-default multi-user.target`.

**Isolate GUI:** `sudo systemctl isolate graphical.target`.

**3. Emergency Mode:**

**Action:** Remounted `/` as `rw` and created `/root/emergency_test.txt`.

**[Insert Screenshot of file creation in emergency shell]**.

**4. Fstab Recovery:**

**Action:** Modified `/etc/fstab` with a typo, rebooted into emergency mode, and corrected the file to restore boot functionality.

**5. Systemd Mount Unit:**

**File:** `/etc/systemd/system/home-user-ram.mount`.

**Activation:** `sudo systemctl start home-user-ram.mount`.

**[Insert Screenshot of `mount | grep ram`]**

## Evaluation Guide

### How Does a Virtual Machine Work?

A virtual machine is software that simulates a physical computer, allowing operating systems and applications to run in isolation on the host system's hardware.

### Difference Between Debian and Rocky

**Debian** - Open-source operating system focused on stability, uses APT as package manager.

**Rocky** - Enterprise-oriented distribution, uses DNF as package manager, binary compatible with RHEL.

### Aptitude vs APT (Advanced Package Tool)

Both are package managers, but Aptitude has a more advanced graphical interface and better support for resolving dependencies automatically.

### What is AppArmor?

A security tool that restricts application access to the system through mandatory access control (MAC).

Check status:
```bash
sudo aa-status
```

### Simple Setup Verification

**Check if graphical interface is installed:**
```bash
ls /usr/bin/*session
```

**Expected output:**
```
/usr/bin/dbus-run-session
/usr/bin/byobu-session
```

**Unwanted output:**
```
/usr/bin/gnome-session
```

**Check UFW:**
```bash
sudo ufw status
```

**Check SSH:**
```bash
sudo systemctl status ssh
```

**Check OS:**
```bash
uname --kernel-version
```

### User Management

**Check if user exists and is in sudo and user42 groups:**
```bash
getent group sudo
getent group user42
```

**Create new user:**
```bash
sudo adduser 
```

**Password policy setup:**

Install required package:
```bash
sudo apt install libpam-pwquality
```

Edit `/etc/pam.d/common-password` and add:
- `retry=3` - Maximum 3 attempts
- `minlen=10` - Minimum 10 characters
- `ucredit=-1` - At least 1 uppercase letter
- `lcredit=-1` - At least 1 lowercase letter
- `dcredit=-1` - At least 1 digit
- `maxrepeat=3` - Maximum 3 consecutive identical characters
- `usercheck=1` - Check if password contains username
- `difok=7` - At least 7 different characters from old password
- `enforce_for_root` - Apply to root user

**View password rules:**
```bash
sudo vim /etc/pam.d/common-password
```

**Password aging policy:**

Edit `/etc/login.defs`:
```bash
sudo vim /etc/login.defs
```

Set password expiration for existing user:
```bash
sudo chage -M 30   # Maximum 30 days
sudo chage -m 2    # Minimum 2 days
sudo chage -W 7    # Warning 7 days before
sudo chage -l      # View password policy
```

**Change password:**
```bash
passwd 
```

**Create a group:**
```bash
sudo groupadd evaluating
```

**Add user to group:**
```bash
sudo usermod -aG evaluating 
```
or
```bash
sudo adduser  evaluating
```

**Verify group membership:**
```bash
getent group evaluating
groups 
```

### Hostname and Partitions

**Check hostname:**
```bash
hostname
```

**Change hostname:**

Method 1:
```bash
sudo su
vim /etc/hostname
# Change the name
reboot
```

Method 2:
```bash
sudo hostnamectl set-hostname 
sudo reboot
```

**View partitions:**
```bash
lsblk
```

**How does LVM work?**

LVM stands for Logical Volume Manager, a technology that manages storage space more flexibly than traditional partitions.

- **Physical Volumes (PV)**: Physical disks or partitions
- **Volume Group (VG)**: Combines physical volumes into a single storage pool
- **Logical Volumes (LV)**: Flexible divisions where data is stored

This allows for dynamic resizing and better disk space management.

### SUDO

**Check sudo installation:**
```bash
which sudo
```

**Add user to sudo group:**
```bash
sudo adduser  sudo
getent group sudo
```

**View sudo configuration:**
```bash
sudo visudo
```

Sudo grants administrative (superuser do) permissions.

**Verify sudo logs:**
```bash
cd /var/log/sudo
ls
cat sudo.log
```

### UFW (Uncomplicated Firewall)

**Check UFW status:**
```bash
sudo service ufw status
```
or
```bash
sudo ufw status
```

**What is UFW?**

Uncomplicated Firewall is a user-friendly interface for managing firewall rules in Debian-based systems. It protects against unauthorized access and attacks.

**List rules:**
```bash
sudo ufw status numbered
```

**Add new rule:**
```bash
sudo ufw allow 8080
```

**Delete rule:**
```bash
sudo ufw delete 
```

### SSH (Secure Shell)

**What is SSH?**

A cryptographic network protocol for secure operation of network services over an unsecured network. Used to access another computer remotely in a secure way.

**Check SSH installation:**
```bash
which ssh
```

**Check if SSH is running:**
```bash
sudo service ssh status
```

**Connect via SSH:**
```bash
ssh user@ip -p 4242
```

**Test root login (should fail):**
```bash
ssh root@ip -p 4242
```

Show in configuration file `/etc/ssh/sshd_config`:
- `Port 4242`
- `PermitRootLogin no`

**Restart SSH after configuration changes:**
```bash
sudo systemctl restart ssh
```
### Monitoring Script

**View crontab:**
```bash
sudo crontab -e
```

Crontab format: `minute hour day_of_month month day_of_week command`

Example for every 10 minutes:
```
*/10 * * * * /path/to/monitoring.sh
```

**Stop cron service:**
```bash
sudo systemctl stop cron
```

**Start cron service:**
```bash
sudo systemctl start cron
```

**Script location:**
```bash
/usr/local/bin/monitoring.sh
```

The script should display:
- System architecture and kernel version
- Number of physical and virtual processors
- Current RAM and utilization percentage
- Current disk space and utilization percentage
- Current CPU load percentage
- Last reboot date and time
- LVM status (active or not)
- Number of active connections
- Number of users logged in
- IPv4 and MAC address
- Number of commands executed with sudo

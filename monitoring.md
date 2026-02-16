# Monitoring Script Explanation

## Script monitoring.sh

### Shebang Line
```bash
#!/bin/bash
```

This line indicates that the script should be executed using the bash interpreter.

---

### 1. System Architecture Information
```bash
arch=$(uname -a)
printf "#Architecture: $arch\n"
```

- `uname -a`: Retrieves information about the kernel and system architecture.
- `arch`: Variable that stores the command result.
- `printf`: Displays the collected architecture.

---

### 2. Number of Physical CPUs
```bash
pcpu=$(cat /proc/cpuinfo | grep "physical id" | sort | uniq | wc -l)
printf "#CPU physical: $pcpu\n"
```

- `/proc/cpuinfo`: System file containing information about CPUs.
- `grep "physical id"`: Filters lines containing physical id (indicates physical CPUs).
- `sort | uniq`: Removes duplicates to count only unique CPUs.
- `wc -l`: Counts the number of physical CPUs.
- `pcpu`: Variable that stores the number of physical CPUs.

---

### 3. Number of Virtual CPUs (vCPUs)
```bash
vcpu=$(cat /proc/cpuinfo | grep "processor" | sort | uniq | wc -l)
printf "#vCPU: $vcpu\n"
```

- `grep "processor"`: Filters lines indicating processors (virtual cores).
- `vcpu`: Variable that stores the number of virtual cores (vCPUs).

---

### 4. RAM Memory Usage
```bash
fram=$(free -m | grep Mem | awk '{print $2}')
uram=$(free -m | grep Mem | awk '{print $3}')
pram=$(free -m | grep Mem | awk '{print $3/$2 * 100}')
printf "#Memory Usage: $uram/$fram%s (%.2f%%)\n" "MB" "$pram"
```

- `free -m`: Shows memory usage in megabytes.
- `grep Mem`: Filters the line with memory information.
- `awk '{print $2}'`: Extracts the total amount of memory (`fram`).
- `awk '{print $3}'`: Extracts the used memory (`uram`).
- `awk '{print $3/$2 * 100}'`: Calculates the memory usage percentage (`pram`).
- `printf`: Displays memory usage in the format used/total (percentage).

---

### 5. Disk Usage
```bash
sdisk=$(df -h --total | awk END'{print $2}' | cut -d G -f1)
udisk=$(df -h --total | awk END'{print $3}' | cut -d G -f1)
pdisk=$(df -h --total | awk END'{print $3/$2 * 100}')
printf "#Disk Usage: $udisk/$sdisk%s (%.2f%%)\n" "Gb" "$pdisk"
```

- `df -h --total`: Shows total disk usage in human-readable format.
- `awk END'{print $2}'`: Gets the total disk space (`sdisk`).
- `awk END'{print $3}'`: Gets the used space (`udisk`).
- `awk END'{print $3/$2 * 100}'`: Calculates the usage percentage (`pdisk`).
- `cut -d G -f1`: Removes the G suffix (gigabytes).
- `printf`: Displays disk usage in the format used/total (percentage).

---

### 6. CPU Load
```bash
cpul=$(mpstat | tail -n 1 | awk '{print 100-$NF}')
printf "#CPU load: %.1f%%\n" "$cpul"
```

- `mpstat`: Shows CPU usage statistics.
- `tail -n 1`: Gets the last line with relevant data.
- `awk '{print 100-$NF}'`: Calculates CPU load (100% minus idle time).
- `cpul`: Variable that stores the CPU load.

---

### 7. Last System Boot
```bash
boot=$(who -b | awk '{print $3" "$4}')
printf "#Last boot: $boot\n"
```

- `who -b`: Shows the last time the system was booted.
- `awk '{print $3" "$4}'`: Extracts the boot date and time.
- `boot`: Variable that stores the last boot information.

---

### 8. LVM Usage Verification
```bash
lvm=$(lsblk | grep "lvm" | wc -l)
if [ $lvm -eq 0 ]
then
    printf "#LVM use: no\n"
else
    printf "#LVM use: yes\n"
fi
```

- `lsblk`: Shows information about block devices.
- `grep "lvm"`: Checks if there are logical volumes (LVM).
- `wc -l`: Counts the number of occurrences.
- Conditional:
  - If `lvm` is 0, displays `LVM use: no`.
  - Otherwise, displays `LVM use: yes`.

---

### 9. Active TCP Connections
```bash
ctcp=$(ss | grep "tcp" | wc -l)
printf "#Connections TCP: $ctcp %s\n" "ESTABLISHED"
```

- `ss`: Shows active connections.
- `grep "tcp"`: Filters TCP connections.
- `wc -l`: Counts the number of active TCP connections.
- `ctcp`: Variable that stores the count.

---

### 10. Number of Logged-in Users
```bash
usr=$(users | wc -w)
printf "#User log: $usr\n"
```

- `users`: Shows currently logged-in users.
- `wc -w`: Counts the number of words (users).
- `usr`: Variable that stores the count of logged-in users.

---

### 11. Network Information (IP and MAC)
```bash
ip=$(hostname -I | awk '{print $1}')
mac=$(ip address | grep "ether" | awk '{print $2}')
printf "#Network: IP $ip ($mac)\n"
```

- `hostname -I`: Gets the system's IP addresses.
- `awk '{print $1}'`: Extracts the first IP.
- `ip address`: Shows information about network interfaces.
- `grep "ether"`: Filters the line with the MAC address.
- `awk '{print $2}'`: Extracts the MAC address.
- `ip` and `mac`: Variables that store IP and MAC addresses.

---

### 12. Number of Sudo Commands Executed
```bash
sudo=$(cat /var/log/sudo/sudo.log | grep "COMMAND" | wc -l)
printf "#Sudo: $sudo\n"
```

- `/var/log/sudo/sudo.log`: Sudo log file.
- `grep "COMMAND"`: Filters lines with commands executed via sudo.
- `wc -l`: Counts the number of commands.
- `sudo`: Variable that stores this count.

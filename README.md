*This project has been created as part of the 42 curriculum by adores.*

# Born2beRoot

## Description
**Born2beRoot** is a system administration project that introduces the fundamentals of virtualization, server configuration, and security hardening. The goal is to set up a virtual machine running a minimal Debian or Rocky Linux installation, configured according to strict security and operational requirements. This project covers topics such as partitioning schemes (LVM), SSH configuration, firewall rules (UFW), password policies, and user management.

## Instructions

### Virtual Machine Setup

1. Download and install a virtualization software (VirtualBox or UTM)
2. Download the Debian or Rocky Linux ISO
3. Create a new virtual machine with:
   - Minimal disk space as required by the subject
   - LVM partitioning scheme
   - Encrypted partitions (bonus)
4. Follow the installation wizard and configure the system according to project requirements

### Key Configuration Steps

- Set up hostname as `<login>42`
- Configure password policy
- Install and configure SSH service on port 4242
- Install and configure UFW firewall
- Set up sudo with strict rules
- Create monitoring script to display system information

## Project Requirements

### Mandatory Part

- Use the latest stable version of Debian or Rocky Linux
- Create at least 2 encrypted partitions using LVM
- SSH service running on port 4242 only
- Configure UFW firewall and allow only port 4242
- Strong password policy implementation
- Strict sudo configuration
- Monitoring script displaying system information every 10 minutes

### Password Policy

- Password expires every 30 days
- Minimum 2 days before password modification
- Warning 7 days before expiration
- Minimum 10 characters
- Must contain uppercase, lowercase, and digit
- Maximum 3 consecutive identical characters
- Must not include username
- Must have at least 7 characters different from old password (not applicable to root)

### Sudo Configuration

- Authentication limited to 3 attempts
- Custom error message for wrong password
- Log all sudo actions (inputs and outputs) to `/var/log/sudo/`
- Enable TTY mode
- Restrict paths usable by sudo

## Project Structure
```
Born2beRoot/
├── monitoring.sh           # System monitoring script
├── signature.txt           # VM signature file
└── README.md              # This file
```

## Resources

The main resource that contributed to my understanding of this project was discussing its requirements and challenges with my colleagues.

Additional references that were helpful include:

- [Born2beRoot Tutorial - hanshazairi](https://youtu.be/s2eM7L_etzo?si=aUAAZ-_im_BcUIdJ)

AI tools were used to assist with the writing and structuring of this README file.

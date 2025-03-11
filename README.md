# Lalatendu Hardened OpenSSH Configuration | Debian

## Author: Lalatendu Swain
**GitHub**: [Lalatenduswain](https://github.com/Lalatenduswain/)  
**Website**: [blog.lalatendu.info](https://blog.lalatendu.info/)  

## Table of Contents
- [Introduction](#introduction)
- [Prerequisites](#prerequisites)
- [Installation](#installation)
- [Configuration Details](#configuration-details)
- [Fail2Ban Setup](#fail2ban-setup)
- [Firewall Configuration](#firewall-configuration)
- [Usage](#usage)
- [Additional Setup](#additional-setup)
- [Crontab Setup](#crontab-setup)
- [Support and Contact](#support-and-contact)
- [Donations](#donations)
- [Disclaimer](#disclaimer)

## Introduction
This repository contains a hardened version of the OpenSSH server (7.4+) configuration file to enhance security, prevent unauthorized access, and mitigate brute-force attacks. It includes advanced security measures such as disabling password authentication, allowing only key-based authentication, and setting up Fail2Ban for intrusion detection.

**Please carefully review the configuration file before applying it.** You are responsible for any actions taken on your system.

## Prerequisites
- Must have `sudo` privileges.
- Ensure you have an SSH key pair (`id_rsa.pub`) generated and added to `~/.ssh/authorized_keys`.
- A backup of your existing SSH configuration:
  ```bash
  sudo cp /etc/ssh/sshd_config "/etc/ssh/sshd_config_backup_$(date +"%Y%m%d%H%M%S")"
  ```

## Installation
1. Download the `sshd_config` file from this repository.
   ```bash
   curl -L https://lalatendu.info/config/sshd_config -o sshd_config
   ```
2. Backup your current SSH configuration:
   ```bash
   sudo cp /etc/ssh/sshd_config /etc/ssh/sshd_config.backup
   ```
3. Replace the old `sshd_config` file with the downloaded one:
   ```bash
   sudo mv sshd_config /etc/ssh/sshd_config
   ```
4. Ensure correct ownership and permissions:
   ```bash
   sudo chown root:root /etc/ssh/sshd_config
   sudo chmod 644 /etc/ssh/sshd_config
   ```
5. Restart the SSH service:
   ```bash
   sudo systemctl restart ssh
   ```

## Configuration Details
### SSH Hardening Features
- Changes SSH port to **6594**
- Disables **root login**
- Enables **key-based authentication only**
- Sets **strict login policies**
- Disables **password authentication**
- Restricts **TCP and agent forwarding**
- Limits **failed login attempts**
- Uses strong **encryption algorithms**

## Deactivating Short Diffie-Hellman Moduli
According to [Mozilla's OpenSSH server hardening guide](https://infosec.mozilla.org/guidelines/openssh#modern-openssh-67), Diffie-Hellman moduli used for `diffie-hellman-group-exchange-sha256` should be at least 3072 bits long. This can be accomplished with the following command:
```bash
cp /etc/ssh/moduli /etc/ssh/moduli.backup
```

## Fail2Ban Setup
To prevent brute-force attacks:
1. Install Fail2Ban:
   ```bash
   sudo apt install fail2ban -y
   ```
2. Create a custom SSH jail:
   ```bash
   sudo nano /etc/fail2ban/jail.local
   ```
   Add:
   ```ini
   [sshd]
   enabled = true
   port = 6594
   filter = sshd
   logpath = /var/log/auth.log
   maxretry = 3
   bantime = 600
   ```
3. Restart Fail2Ban:
   ```bash
   sudo systemctl restart fail2ban
   ```

## Firewall Configuration
To allow SSH on port 6594:
```bash
sudo ufw allow 6594/tcp
sudo firewall-cmd --permanent --add-port=6594/tcp  # For RHEL
sudo firewall-cmd --reload  # For RHEL
sudo iptables -A INPUT -p tcp --dport 6594 -j ACCEPT
sudo service iptables save
```

## Usage
- Ensure SSH is running:
  ```bash
  sudo systemctl status sshd
  ```
- Connect using:
  ```bash
  ssh -p 6594 lalatendu@your-server-ip
  ```

## Additional Setup
- Set up TCP Wrappers (`/etc/hosts.allow` & `/etc/hosts.deny`) to restrict SSH access by IP.

## Crontab Setup (Optional)
To periodically check SSH logs for suspicious activity:
```bash
crontab -e
```
Add:
```bash
@daily cat /var/log/auth.log | grep "Failed password" >> /var/log/ssh_attempts.log
```

## Support and Contact
Encountering issues? Submit an issue on GitHub:
[GitHub Issues](https://github.com/Lalatenduswain/hardened-ssh-config/issues)

## Donations
If you find this script useful and want to show your appreciation, you can donate via [Buy Me a Coffee](https://www.buymeacoffee.com/lalatendu.swain).

## Disclaimer
This script is provided "as-is" and may require modifications or updates based on your specific environment and requirements. Use at your own risk. The authors of the script are not liable for any damages or issues caused by its usage.

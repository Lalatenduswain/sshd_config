# Harden OpenSSH Configuration

## Introduction

This repository contains a hardened OpenSSH configuration designed to enhance security for SSH connections. The configuration settings are tailored to reduce exposure to automated scans, prevent unauthorized access, and mitigate various security threats.

## Configuration Details

### Features

- Utilizes SSH key-based authentication instead of password-based authentication.
- Disables root login via SSH to prevent direct root access.
- Limits SSH access to specific users.
- Restricts SSH access from certain IP addresses.
- Sets maximum authentication lifetime, login grace time, and number of authentication attempts per connection to mitigate brute-force attacks.
- Disables TCP port forwarding over SSH tunnels.
- Enforces strict mode for enhanced security.

### Authentication

- Only allows specified users to log in.
- Denies login for specific users and groups.
- Disables host-based authentication.
- Implements TCP wrapper to control access based on IP addresses or hostnames.

## Disclaimer | Running the Script

**Author:** Lalatendu Swain | [GitHub](https://github.com/Lalatenduswain) | [Website](https://blog.lalatendu.info/)

This script is provided as-is and may require modifications or updates based on your specific environment and requirements. Use it at your own risk. The authors of the script are not liable for any damages or issues caused by its usage.

## Donations

If you find this script useful and want to show your appreciation, you can donate via [Buy Me a Coffee](https://www.buymeacoffee.com/lalatendu.swain).

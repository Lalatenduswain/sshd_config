# OpenSSH Configuration Hardening

This repository contains a hardened version of the OpenSSH server (7.4+) configuration file.

**Please carefully review the configuration file before applying it.** You are responsible for any actions taken on your system.

## Usage

1. Download the `sshd_config` file from this repository.
2. **Review the contents of the `sshd_config` file to ensure all settings are suitable for your system.**
3. Backup your current `/etc/ssh/sshd_config` file.
4. Replace the old `sshd_config` file with the downloaded one.
5. Restart the SSH service using the appropriate command (e.g., `sudo systemctl restart ssh`).

```shell
# Download the configuration file from GitHub using curl or other methods
curl https://raw.githubusercontent.com/lalatenduswain/sshd_config/master/sshd_config -o ~/sshd_config
```
Or

```shell
curl -L https://lalatendu.info/config/sshd_config -o sshd_config
```

```shell
# Backup the original sshd_config
sudo cp /etc/ssh/sshd_config /etc/ssh/sshd_config.backup
```

```shell
# Replace the old sshd_config with the new one
sudo mv ~/sshd_config /etc/ssh/sshd_config
```

```shell
# Ensure the file has the correct ownership and permissions
sudo chown root:root /etc/ssh/sshd_config
sudo chmod 644 /etc/ssh/sshd_config
```

```shell
# Use systemctl to reload the SSH server and apply the new configurations
# On some distributions, the SSH server service is called sshd
sudo systemctl restart ssh
```

For convenience, the URL `https://lalatendu.info/config/sshd_config` points to the `sshd_config` file. You may download the `sshd_config` file using the following command. However, be sure to verify the integrity of the file after downloading if you choose this method.

It is recommended to use the [ssh-audit](https://github.com/jtesta/ssh-audit) script to check the cryptographic strength of your SSH server after configuring it.

## Deactivating Short Diffie-Hellman Moduli

According to [Mozilla's OpenSSH server hardening guide](https://infosec.mozilla.org/guidelines/openssh#modern-openssh-67), Diffie-Hellman moduli used for `diffie-hellman-group-exchange-sha256` should be at least 3072 bits long. This can be accomplished with the following commands.

```shell
# Backup the original moduli file
cp /etc/ssh/moduli /etc/ssh/moduli.backup
```

## Disclaimer | Running the Script

**Author:** Lalatendu Swain | [GitHub](https://github.com/Lalatenduswain) | [Website](https://blog.lalatendu.info/)

This script is provided as-is and may require modifications or updates based on your specific environment and requirements. Use it at your own risk. The authors of the script are not liable for any damages or issues caused by its usage.

## Donations

If you find this script useful and want to show your appreciation, you can donate via [Buy Me a Coffee](https://www.buymeacoffee.com/lalatendu.swain).

# Harden OpenSSH Configuration

This repository hosts my hardened version of OpenSSH server (7.4+) configuration file.

**Please review the configuration file carefully before applying it.** You are responsible for actions done to your own system.

## Usages

1. Download the file `sshd_config` from the repository
1. **Review the content of the `sshd_config` file to make sure all settings are suitable for your system**
1. Backup your current `/etc/ssh/sshd_config` file
1. Overwrite the old `sshd_config` file with the downloaded `sshd_config` file
1. Run the appropriate command to restart the SSH service (e.g., `sudo systemctl restart ssh`)

```shell
# download the configuration file from GitHub using curl or other methods
curl https://raw.githubusercontent.com/lalatenduswain/sshd_config/master/sshd_config -o ~/sshd_config
```

```shell
# backup the original sshd_config
sudo cp /etc/ssh/sshd_config /etc/ssh/sshd_config.backup
```

```shell
# replace the old sshd_config with the new one
sudo mv ~/sshd_config /etc/ssh/sshd_config
```

```shell
# make sure the file has the correct ownership and permissions
sudo chown root:root /etc/ssh/sshd_config
sudo chmod 644 /etc/ssh/sshd_config
```

```shell
# use systemctl to reload the SSH server and apply the new configurations
# on some distributions the SSH server service is called sshd
sudo systemctl restart ssh
```

For convenience, I have pointed the URL `https://k4t.io/sshd` to the `sshd_config` file. You may therefore download the `sshd_config` file with the following command. However, be sure to check the integrity of the file after downloading it if you choose to download using this method.

```shell
curl -L https://lalatendu.info/config/sshd_config -o sshd_config
```

It's recommended to use the [ssh-audit](https://github.com/jtesta/ssh-audit) script to check the cryptographic strength of your SSH server after done configuring it.

## Deactivating Short Diffie-Hellman Moduli

Diffie-Hellman moduli used for `diffie-hellman-group-exchange-sha256` should be at lest 3072 bits long according to [Mozilla's OpenSSH server hardening guide](https://infosec.mozilla.org/guidelines/openssh#modern-openssh-67). This can be done with the following commands.

```shell
# backup original moduli file
cp /etc/ssh/moduli /etc/ssh/moduli.backup
```

## Disclaimer | Running the Script

**Author:** Lalatendu Swain | [GitHub](https://github.com/Lalatenduswain) | [Website](https://blog.lalatendu.info/)

This script is provided as-is and may require modifications or updates based on your specific environment and requirements. Use it at your own risk. The authors of the script are not liable for any damages or issues caused by its usage.

## Donations

If you find this script useful and want to show your appreciation, you can donate via [Buy Me a Coffee](https://www.buymeacoffee.com/lalatendu.swain).

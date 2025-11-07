sudo mkdir -p /etc/systemd/system/ssh.socket.d
sudo tee /etc/systemd/system/ssh.socket.d/listen.conf >/dev/null <<'EOF'
[Socket]
ListenStream=
ListenStream=1234
EOF

sudo systemctl daemon-reload
sudo systemctl status ssh.socket

sudo ss -tuln | grep 1234


sudo ufw allow 1234/tcp comment "SSH alternate port 1234"
sudo ufw reload
sudo systemctl restart sshd



#############################################################################
Include /etc/ssh/sshd_config.d/*.conf
Port 1234
UsePAM yes
PrintMotd no
MaxAuthTries 6
DenyUsers ubuntu
X11Forwarding yes
AcceptEnv LANG LC_*
PermitRootLogin yes
PubkeyAuthentication yes
PasswordAuthentication no
KbdInteractiveAuthentication no
PubkeyAcceptedAlgorithms +ssh-rsa
AllowUsers root yohannan lalatendu
Subsystem sftp /usr/lib/openssh/sftp-server

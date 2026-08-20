# <img src="roadmap.png" width="35" align="top" /> Linux Server Setup

Linux Server project from [roadmap.sh](https://roadmap.sh/projects/linux-server-setup) Backend Developer Roadmap. 

## 📝 About the Project

A Linux server configured with essential security measures and settings that every production server should have. For this project, I used Linode as the host.

## 🛠️ Setup
1. Create a non-root user with sudo privileges.
```
add your_new_username
usermod -aG sudo your_new_username
```
2. Set up SSH key-based authentication and disable password-based login.
```
ssh-keygen -t ed25519 -C "name_of_key"
ssh-copy-id -i ~/.ssh/id_ed25519.pub username@server-address
ssh username@server-address
```
3. Set up UFW to allow only SSH (port 22) by default. Check that ufw is working with `sudo ufw status verbose`.
```
sudo apt update
sudo apt install ufw
sudo ufw allow 22/tcp
sudo ufw enable
```
4. Update all system packages and configure automatic security updates using unattended-upgrades.
```
# Refresh package information
sudo apt update

# Install current updates and reboot
sudo apt upgrade
sudo reboot

# Install automatic-update support
sudo apt install unattended-upgrades

# Configure automatic security updates
sudo dpkg-reconfigure unattended-upgrades

# Verify configuration
cat /etc/apt/apt.conf.d/20auto-upgrades
```
5. Install and configure Fail2Ban to protect against brute-force SSH attacks.
```
sudo apt update
sudo apt install fail2ban

#verify fail2ban is running
sudo systemctl status fail2ban


#create an ssh jail
sudo nano /etc/fail2ban/jail.local

#add 
[sshd]
enabled = true
port = 22
filter = sshd
backend = systemd

maxretry = 5
findtime = 10m
bantime = 1h


#check configuration
sudo fail2ban-client -t

#restart
sudo systemctl restart fail2ban

#verify everythings working
sudo systemctl status fail2ban
sudo fail2ban-client status
```
6. Set the correct timezone and a meaningful hostname.
```
sudo timedatectl set-timezone America/Los_Angeles
sudo hostnamectl set-hostname new_server_name
```
7. Demonstrate basic systemctl commands.
```
#check status of a service 
sudo systemctl status SERVICE

#stop service
sudo systemctl stop SERVICE

#start service
sudo systemctl start SERVICE

#restart service
sudo systemctl restart SERVICE

#enable service at boot
sudo systemctl enable SERVICE

#prevent from automatically starting after a reboot
sudo systemctl disable SERVICE
```
8. Use journalctl to view system logs and locate common log files in /var/log/.
```
# 1. View logs from this boot
sudo journalctl -b

# 2. View SSH service logs
sudo journalctl -u ssh --since "today"

# 3. Locate traditional Linux log files
sudo ls -lah /var/log/

# 4. Inspect authentication/SSH logs
sudo tail -n 50 /var/log/auth.log

# 5. Inspect Fail2Ban logs
sudo journalctl -u fail2ban
```
9. Complete a security checklist confirming all configurations are in place and working correctly.
```
whoami
sudo whoami
sudo sshd -T | grep -E 'passwordauthentication|pubkeyauthentication'
sudo ufw status verbose
sudo systemctl is-active ssh
sudo systemctl is-active fail2ban
sudo fail2ban-client status sshd
hostnamectl
timedatectl
sudo journalctl -u ssh --since "today"
```

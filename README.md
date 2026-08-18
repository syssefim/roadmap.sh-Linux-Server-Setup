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
```

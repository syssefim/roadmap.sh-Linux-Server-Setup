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


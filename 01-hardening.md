# 01 — Linux Server Hardening

## Objective

Configure a secure baseline on a fresh Ubuntu 22.04 ARM64 server hosted on Oracle Cloud.

## 1. System Update
```bash
sudo apt update && sudo apt upgrade -y
```

## 2. Create Non-Root User
```bash
sudo adduser mariano
sudo usermod -aG sudo mariano
```

## 3. Copy SSH Access to New User
```bash
sudo mkdir /home/mariano/.ssh
sudo cp ~/.ssh/authorized_keys /home/mariano/.ssh/
sudo chmod 700 /home/mariano/.ssh
sudo chmod 600 /home/mariano/.ssh/authorized_keys
sudo chown -R mariano:mariano /home/mariano/.ssh
```

## 4. Harden SSH Configuration

Edit `/etc/ssh/sshd_config`:
```
PermitRootLogin no
PasswordAuthentication no
```
```bash
sudo systemctl restart ssh
```

**Why:** Disabling root login and password authentication means the only way to access the server is with a physical SSH key. No key = no entry.

## 5. Configure UFW Firewall
```bash
sudo apt install ufw -y
sudo ufw allow OpenSSH
sudo ufw enable
```

**Why:** A freshly created server accepts connections on all ports by default. UFW acts as a gatekeeper — only explicitly allowed ports are accessible.

## 6. Install Fail2ban
```bash
sudo apt install fail2ban -y
sudo systemctl enable fail2ban
sudo systemctl start fail2ban
```

**Why:** Automatically bans IPs that repeatedly fail authentication attempts. Protects against brute-force attacks.

## 7. Timezone and Clock Sync
```bash
sudo timedatectl set-timezone America/Argentina/Buenos_Aires
sudo apt install chrony -y
sudo systemctl enable chrony
sudo systemctl start chrony
```

**Why:** Accurate timestamps are critical for log analysis and security event correlation.

## Result

- Non-root user with SSH key-only access
- Root login disabled
- Password authentication disabled
- Firewall active with minimal open ports
- Brute-force protection enabled
- Clock synchronized

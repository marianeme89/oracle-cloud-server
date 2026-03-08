# 02 — Docker Installation

## Objective

Install Docker Engine on Ubuntu 22.04 ARM64 using the official Docker repository.

## Why Docker

Docker allows running applications in isolated containers. Each service has everything it needs to run without interfering with the rest of the system. This makes deployment, updates and rollbacks simple and reproducible.

## Installation

### 1. Install dependencies
```bash
sudo apt install ca-certificates curl gnupg -y
```

### 2. Add Docker's official GPG key
```bash
sudo install -m 0755 -d /etc/apt/keyrings
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /etc/apt/keyrings/docker.gpg
sudo chmod a+r /etc/apt/keyrings/docker.gpg
```

### 3. Add Docker repository
```bash
echo \
  "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.gpg] \
  https://download.docker.com/linux/ubuntu \
  $(. /etc/os-release && echo "$VERSION_CODENAME") stable" | \
  sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
```

### 4. Install Docker Engine
```bash
sudo apt update
sudo apt install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin -y
```

### 5. Add user to docker group
```bash
sudo usermod -aG docker mariano
```

**Why:** Allows running Docker commands without sudo. Requires logout/login to take effect.

### 6. Verify installation
```bash
docker run hello-world
```

## Result

- Docker Engine running on ARM64
- User added to docker group
- Docker Compose available as a plugin

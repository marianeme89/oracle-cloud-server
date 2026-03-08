# 05 — Services Dashboard with Homepage

## Objective

Deploy a central dashboard to access all self-hosted services from a single interface.

## Why Homepage

Homepage provides a clean, lightweight start page with real-time server stats (CPU, RAM, disk) and quick access to all running services.

## Installation
```bash
docker run -d \
  --name homepage \
  --restart=always \
  -p 3000:3000 \
  -e HOMEPAGE_ALLOWED_HOSTS=129.213.136.45:3000 \
  -v /home/mariano/homepage:/app/config \
  ghcr.io/gethomepage/homepage:latest
```
```bash
sudo ufw allow 3000/tcp
```

> `HOMEPAGE_ALLOWED_HOSTS` is required when accessing via IP. Without it, Homepage blocks the request with a host validation error.

## Open Port in Oracle Cloud

Add Ingress Rule in Security Lists:
- Source CIDR: `0.0.0.0/0`
- IP Protocol: `TCP`
- Destination Port Range: `3000`

## Services Configuration

Edit `/home/mariano/homepage/services.yaml`:
```bash
sudo chown -R mariano:mariano /home/mariano/homepage

cat > /home/mariano/homepage/services.yaml << 'EOF'
- Monitoring:
    - Dashdot:
        href: http://SERVER_IP:3001
        description: Server stats
    - Portainer:
        href: http://SERVER_IP:9000
        description: Docker management

- Automation:
    - N8N:
        href: http://SERVER_IP:5678
        description: Workflow automation

- VPN:
    - WireGuard:
        href: http://SERVER_IP:51821
        description: VPN admin panel
EOF
```

## Access

`http://129.213.136.45:3000`

## Result

- Central dashboard with all services organized by category
- Real-time CPU, RAM and disk stats in the header
- One-click access to Portainer, N8N, Dashdot and WireGuard

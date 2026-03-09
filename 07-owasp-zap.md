# 07 — OWASP ZAP Web Vulnerability Scanner

## Objective

Deploy OWASP ZAP (Zed Attack Proxy) for web application vulnerability scanning and security testing.

## What is OWASP ZAP

ZAP is one of the world's most popular free security tools, maintained by the Open Worldwide Application Security Project (OWASP). 
It is used by pentesters and security professionals to find vulnerabilities in web applications.

## Installation
```bash
docker run -d \
  --name zap \
  --restart=always \
  -p 8080:8080 \
  -p 8090:8090 \
  zaproxy/zap-stable zap-webswing.sh
```
```bash
sudo ufw allow 8080/tcp
```

Add port 8080 TCP as Ingress Rule in Oracle Cloud Security Lists.

## Access

`http://SERVER_IP:8080/zap`

## Use Cases

- Scan web applications for common vulnerabilities (OWASP Top 10)
- Intercept and inspect HTTP/HTTPS traffic
- Automated and manual security testing
- Generate vulnerability reports

## Result

- ZAP running as a persistent Docker container
- Accessible via browser with full GUI
- Ready for web application security testing

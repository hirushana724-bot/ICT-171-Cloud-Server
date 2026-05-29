# ICT171 Cloud Server Project — Sri Lanka Tourism Website

**Student Name:** Your Full Name  
**Student Number:** Your Student Number  
**GitHub Repo:** https://github.com/hirushana724-bot/ICT-171-Cloud-Server  
**Live Server IP:** 3.104.9.21  
**DNS Entry:** visitsrilanka.mooo.com  
**Video Explainer:** (add YouTube link after recording)  

---

## Overview
A Sri Lanka tourism website hosted on Ubuntu 22.04 LTS via AWS EC2 (IaaS).  
Built using Apache2 web server with HTML and CSS.  
Secured with SSL/TLS via Certbot from Let's Encrypt.  
The site includes Home, Destinations, Food & Culture and Contact pages.

---

## Step 1 – Launching the EC2 Instance

- Logged into AWS Console
- Navigated to EC2 → Launch Instance
- Selected **Ubuntu Server 22.04 LTS**
- Instance type: **t3.micro** (free tier eligible)
- Configured Security Group to allow:
  - Port 22 (SSH)
  - Port 80 (HTTP)
  - Port 443 (HTTPS)
- Launched instance successfully

---

## Step 2 – Connecting via SSH

Connected to the server using AWS EC2 Instance Connect
directly from the AWS Console browser terminal:

1. Go to AWS Console → EC2 → Instances
2. Select the instance
3. Click **Connect** → **EC2 Instance Connect**
4. Click **Connect**

This opens a browser-based terminal connected to the Ubuntu server.

---

## Step 3 – Installing Apache Web Server

```bash
sudo apt update
sudo apt install apache2 -y
sudo systemctl start apache2
sudo systemctl enable apache2
sudo systemctl status apache2
```

Verified Apache was running by visiting `http://3.104.9.21` in browser.
Apache default page confirmed successful installation.

---

## Step 4 – Deploying the Website

Created tourism website files in `/var/www/html/`:

```bash
cd /var/www/html
sudo rm index.html
sudo nano index.html
sudo nano destinations.html
sudo nano culture.html
sudo nano contact.html
sudo mkdir css
sudo nano css/style.css
```

Website includes four pages:
- **Home** – Full screen hero banner and highlights section
- **Destinations** – Top Sri Lanka destinations with photos
- **Food & Culture** – Sri Lankan food, culture and festivals
- **Contact** – Contact form with full screen background image

---

## Step 5 – Elastic IP Setup

- Navigated to EC2 → Network & Security → Elastic IPs
- Clicked **Allocate Elastic IP address**
- Associated Elastic IP `3.104.9.21` with instance `i-0e1ed9123a6a5612a`
- Server IP is now permanent and does not change on restart

---

## Step 6 – DNS Configuration

- Registered free account at [freedns.afraid.org](https://freedns.afraid.org)
- Created A record:

| Type | Subdomain | Domain | Destination |
|---|---|---|---|
| A | visitsrilanka | mooo.com | 3.104.9.21 |

- Site accessible at: `http://visitsrilanka.mooo.com`

---

## Step 7 – SSL/TLS Setup

```bash
sudo apt update
sudo apt install certbot python3-certbot-apache -y
sudo certbot --apache -d visitsrilanka.mooo.com
sudo certbot renew --dry-run
```

Site now secured at: `https://visitsrilanka.mooo.com` ✅

---

## Step 8 – Bash Script

**Script location:** `/home/ubuntu/sysinfo.sh`  
**Purpose:** Collects and logs server system information including
date/time, hostname, public IP, uptime, disk usage,
memory usage and Apache status.
Output is saved to `/home/ubuntu/sysinfo.log`.

```bash
#!/bin/bash
# =========================================
# Script:  sysinfo.sh
# Author:  Your Name - Student Number
# Purpose: Collects server system info and
#          saves a timestamped log file
# =========================================

LOGFILE="/home/ubuntu/sysinfo.log"

echo "======================================" | tee -a $LOGFILE
echo "   Sri Lanka Tourism Server Report"    | tee -a $LOGFILE
echo "======================================" | tee -a $LOGFILE
echo "Date/Time  : $(date)"                  | tee -a $LOGFILE
echo "Hostname   : $(hostname)"              | tee -a $LOGFILE
echo "Public IP  : $(curl -s ifconfig.me)"  | tee -a $LOGFILE
echo "Uptime     : $(uptime -p)"             | tee -a $LOGFILE
echo "-- Disk Usage --"                      | tee -a $LOGFILE
df -h /                                      | tee -a $LOGFILE
echo "-- Memory Usage --"                    | tee -a $LOGFILE
free -h                                      | tee -a $LOGFILE
echo "-- Apache Status --"                   | tee -a $LOGFILE
systemctl is-active apache2                  | tee -a $LOGFILE
echo "======================================" | tee -a $LOGFILE
```

To run:

```bash
bash /home/ubuntu/sysinfo.sh
cat /home/ubuntu/sysinfo.log
```

---

## References

[1] Amazon Web Services, "Amazon EC2 Documentation," AWS, 2026.  
[Online]. Available: https://docs.aws.amazon.com/ec2

[2] Canonical Ltd., "Install and Configure Apache," Ubuntu Tutorials, 2026.  
[Online]. Available: https://ubuntu.com/tutorials/install-and-configure-apache

[3] Electronic Frontier Foundation, "Certbot Documentation," Certbot, 2026.  
[Online]. Available: https://certbot.eff.org

[4] J. Anderson, "Free DNS," FreeDNS Afraid, 2026.  
[Online]. Available: https://freedns.afraid.org

[5] Pexels, "Free Stock Photos," Pexels, 2026.  
[Online]. Available: https://pexels.com

[6] Wikimedia Foundation, "Wikipedia Free Encyclopedia," Wikipedia, 2026.  
[Online]. Available: https://wikipedia.org

[7] W3Schools, "HTML Tutorial," W3Schools, 2026.  
[Online]. Available: https://www.w3schools.com/html

[8] W3Schools, "CSS Tutorial," W3Schools, 2026.  
[Online]. Available: https://www.w3schools.com/css

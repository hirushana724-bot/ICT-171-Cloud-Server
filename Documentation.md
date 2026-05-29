# ICT171 Cloud Server Project — Sri Lanka Tourism Website

**Student Name:** Anjana Hirushan Perera  
**Student Number:** 35766077  
**GitHub Repo:** https://github.com/hirushana724-bot
**Live Server IP:** 3.104.9.21  
**DNS Entry:** visitsrilanka.mooo.com  
**Video Explainer:** https://youtu.be/GEjhQUI8jLw

---

## Overview
A Sri Lanka tourism website hosted on Ubuntu 22.04 LTS via AWS EC2 (IaaS).  
Built using Apache2 web server with HTML and CSS.  
Secured with SSL/TLS via Certbot from Let's Encrypt.  
The site includes Home, Destinations, Food & Culture and Contact pages.  
The goal of this project is to demonstrate the ability to deploy and configure  
a cloud-based web server from scratch using Infrastructure as a Service.

---

## Step 1 – Launching the EC2 Instance

Logged into AWS Console and navigated to EC2 to launch a new instance.

- Selected **Ubuntu Server 22.04 LTS** as the operating system
- Chose **t3.micro** instance type (free tier eligible)
- Created a new key pair for SSH access
- Configured Security Group inbound rules:

| Port | Protocol | Purpose |
|------|----------|---------|
| 22   | TCP      | SSH access |
| 80   | TCP      | HTTP web traffic |
| 443  | TCP      | HTTPS secure traffic |

- Launched the instance successfully
- Instance ID: `i-0e1ed9123a6a5612a`

---

## Step 2 – Connecting via SSH

Connected to the server using AWS EC2 Instance Connect
directly from the AWS Console browser terminal:

1. Go to AWS Console → EC2 → Instances
2. Select the running instance
3. Click **Connect** → **EC2 Instance Connect**
4. Click **Connect**

This opens a browser-based terminal with direct command line
access to the Ubuntu server. All configuration was done
through this terminal.

---

## Step 3 – Installing Apache Web Server

Updated package list and installed Apache2:

```bash
sudo apt update
sudo apt install apache2 -y
sudo systemctl start apache2
sudo systemctl enable apache2
sudo systemctl status apache2
```

Verified Apache was running by visiting `http://3.104.9.21`
in the browser. The Apache default page confirmed
successful installation.

---

## Step 4 – Deploying the Tourism Website

Navigated to the Apache web root directory and created
all website files:

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

Website structure:

| File | Description |
|------|-------------|
| index.html | Home page with hero banner and highlights |
| destinations.html | Top Sri Lanka destinations with photos |
| culture.html | Food, culture, heritage and festivals |
| contact.html | Contact form with full screen background |
| css/style.css | Shared stylesheet for all pages |

All pages share a common navigation bar, colour scheme
(maroon #8B0000 and gold #FFD700) and footer.
Images were sourced from Pexels and Wikipedia [5][6].

---

## Step 5 – Elastic IP Setup

To ensure the server IP address never changes on restart,
an Elastic IP was allocated and associated:

1. Go to EC2 → Network & Security → Elastic IPs
2. Click **Allocate Elastic IP address** → **Allocate**
3. Select the new IP → **Actions** → **Associate Elastic IP address**
4. Select instance `i-0e1ed9123a6a5612a` → **Associate**

- **Permanent Elastic IP:** `3.104.9.21`

---

## Step 6 – DNS Configuration

Registered a free account at FreeDNS and created
an A record pointing the subdomain to the Elastic IP [4]:

1. Go to [freedns.afraid.org](https://freedns.afraid.org)
2. Sign up for a free account
3. Click **Subdomains** → **Add**
4. Fill in the following:

| Field | Value |
|-------|-------|
| Type | A |
| Subdomain | visitsrilanka |
| Domain | mooo.com |
| Destination | 3.104.9.21 |

5. Click **Save**

Site accessible at: `http://visitsrilanka.mooo.com`

---

## Step 7 – SSL/TLS Setup

Installed Certbot and obtained a free SSL certificate
from Let's Encrypt to enable HTTPS [3]:

```bash
sudo apt update
sudo apt install certbot python3-certbot-apache -y
sudo certbot --apache -d visitsrilanka.mooo.com
sudo certbot renew --dry-run
```

During installation:
- Entered email address for certificate renewal notices
- Agreed to terms of service
- Certbot automatically configured Apache for HTTPS

Site now secured at: `https://visitsrilanka.mooo.com` ✅

To verify the certificate:
```bash
sudo certbot certificates
```

---

## Step 8 – Bash Script

**Script name:** `sysinfo.sh`  
**Location:** `/home/ubuntu/sysinfo.sh`  
**Log file:** `/home/ubuntu/sysinfo.log`  

**Purpose:** This script collects server system information
and saves a timestamped report to a log file. It displays
the date and time, hostname, public IP address, server uptime,
disk usage, memory usage and Apache web server status.
The output is both printed to the terminal and appended
to a persistent log file for future reference.

```bash
#!/bin/bash
# =========================================
# Script:  sysinfo.sh
# Author:  Anjana Hirushan Perera - 35766077
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

To run the script:

```bash
chmod +x /home/ubuntu/sysinfo.sh
bash /home/ubuntu/sysinfo.sh
cat /home/ubuntu/sysinfo.log
```

Sample output:
======================================
Sri Lanka Tourism Server Report
Date/Time  : Fri May 29 12:33:30 UTC 2026
Hostname   : ip-172-31-9-71
Public IP  : 3.104.9.21
Uptime     : up 1 day, 35 minutes
-- Disk Usage --
Filesystem      Size  Used Avail Use% Mounted on
/dev/root       6.7G  2.4G  4.3G  36% /
-- Memory Usage --
total   used   free
Mem:           908Mi  357Mi  121Mi
-- Apache Status --
active

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



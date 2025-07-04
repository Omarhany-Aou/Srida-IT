#صلي على النبي 
# 🚀 Phase 1: Setup Steps — E-commerce Security Lab

📄 **This document outlines the step-by-step procedure** for setting up a simulated e-commerce environment using VirtualBox, Ubuntu, and OpenCart as part of the 45-day cybersecurity internship project.

---

## 🖥️ 1. System Requirements

* **Host OS:** 🪟 Windows 11
* **RAM:** 💾 8GB minimum (**12GB+ recommended**)
* **Virtualization:** ✅ Enabled in BIOS
* **Tools Used:**

  * 🧰 VirtualBox
  * 🐧 Ubuntu 22.04 LTS
  * 🛒 OpenCart 4.0.2.3

---

## ⚙️ 2. Environment Setup

### 📦 Step 1: Install VirtualBox

🔗 Download and install VirtualBox from the official website:
[https://www.virtualbox.org/](https://www.virtualbox.org/)

### 🧱 Step 2: Set Up a Simulated E-Commerce Environment using OpenCart

#### 🔄 Step 2.1: Update and Upgrade Packages

```bash
sudo apt update && sudo apt upgrade -y
```

#### 🛠️ Step 2.2: Install Required Packages

```bash
sudo apt install apache2 php php-mysql mysql-server unzip -y
```

#### 🗃️ Step 2.3: Create the Database

```bash
sudo mysql -u root -p
```

Inside the MySQL shell:

```sql
CREATE DATABASE opencart;
CREATE USER 'opencartuser'@'localhost' IDENTIFIED BY 'StrongPassword123';
GRANT ALL PRIVILEGES ON opencart.* TO 'opencartuser'@'localhost';
FLUSH PRIVILEGES;
EXIT;
```

#### 📥 Step 2.4: Install OpenCart

```bash
cd /var/www/html
sudo rm index.html
sudo wget https://github.com/opencart/opencart/releases/download/4.0.2.3/upload.zip
sudo unzip upload.zip
sudo chown -R www-data:www-data /var/www/html
sudo chmod -R 755 /var/www/html
```

#### 🌐 Step 2.5: Access OpenCart Installer in Browser

🔍 Find your VM IP address:

```bash
hostname -I
# or
ip addr show eth0
```

🖥️ Then open in browser:

```
http://<your-ip-address>
```

➡️ Follow the GUI installation steps in the browser.

---

## 🧯 3. Troubleshooting Apache2 & MySQL on WSL

If `systemctl` doesn’t work in WSL, use:

```bash
# Apache2
sudo service apache2 start
sudo service apache2 status

# MySQL
sudo service mysql start
sudo service mysql status
```

### ❗ Common Error: Database Link Failure

**🔴 Error Message:**
`No such file or directory` in `mysqli.php` line 40

**⚠️ Possible Causes:**

* 🚫 MySQL is not running
* ❌ Incorrect credentials or socket error

✅ To verify MySQL manually:

```bash
sudo service mysql start
mysql -u opencartuser -p
```

Make sure the installation form uses:

* **Host:** `<your-ip-address>`
* **User:** `opencartuser`
* **Password:** `StrongPassword123`
* **Database:** `opencart`

---

## 📚 4. Self-Study: Cybersecurity Fundamentals

🎯 **Focus Areas:**

* 🛡️ OWASP Top 10
* 🛍️ Common e-commerce vulnerabilities
* 🔐 Authentication & session management
* 🐞 SQL Injection, XSS, CSRF

---

## 🧪 5. Install Open-Source Tools

### 🧰 Tool: OpenVAS (Greenbone Vulnerability Manager)

```bash
sudo apt update
sudo apt install -y gvm
sudo gvm-setup
sudo gvm-check-setup
sudo gvm-start
```

🌐 Access it via:

```
https://127.0.0.1:9392
```

### ⚙️ Enabling systemd in WSL (If Needed)

Open PowerShell:

```powershell
notepad $env:USERPROFILE\.wslconfig
```

Add:

```ini
[wsl2]
systemd=true
```

Then:

```powershell
wsl --shutdown
```

Back in Ubuntu:

```bash
ps -p 1 -o comm=
# Should output: systemd
```

If not:

```bash
sudo nano /etc/wsl.conf
```

Add:

```ini
[boot]
systemd=true
```

💾 Save & exit, then in PowerShell:

```powershell
wsl --shutdown
```

🔁 Reopen Ubuntu and confirm:

```bash
ps -p 1 -o comm=
```

### 🟢 Final Step: Start OpenVAS

```bash
sudo systemctl start postgresql
sudo gvm-setup
```

🖥️ Open in browser:

```
https://127.0.0.1:9392
```

---

✅ phase 1 has done !!

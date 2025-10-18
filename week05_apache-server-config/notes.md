# 🌐 Apache, Firewall (UFW), Networking & Hosts File 

### `systemctl` (service management)
- `sudo systemctl status` — shows status of all systemd services 
- `sudo systemctl status apache2` — show Apache service status.
- `sudo systemctl start apache2` — start Apache service.
- `sudo systemctl stop apache2` — stop Apache.
- `sudo systemctl restart apache2` — restart Apache.
- `sudo systemctl reload apache2` — reload config without dropping connections.
- `sudo systemctl enable apache2` — enable Apache to start at boot.
- `sudo systemctl disable apache2` — disable Apache from starting at boot.

**Tip:** Use `sudo apache2ctl configtest` (or `apachectl -t`) to verify Apache config syntax before reloading/restarting.

---

## 🖥 Apache Version Check
```bash
apache2 -v
# Example output:
# Server version: Apache/2.4.58 (Ubuntu)
# Server built:   2025-08-11T11:10:09
```

---

## 🌐 Networking — find IP addresses & interfaces

- `ip a` — show all network interfaces and their IP addresses (replacement for deprecated `ifconfig`).
- `hostname` — show the system hostname.
- `hostname -I` — show the IP addresses assigned to the host.
- `curl -I http://localhost` — fetch HTTP headers from local web server to verify Apache is responding.
- `ss -tuln` or `netstat -tuln` — list listening TCP/UDP ports (useful to check port 80/443).

**Example:** check Apache listening:
```bash
ss -tuln | grep :80
```

---

## 🔥 Firewall (UFW) — Basic Workflow

Ubuntu often ships with UFW (Uncomplicated Firewall) available but disabled by default.

Check status:
```bash
sudo ufw status
# Status: inactive
```

Allow SSH (important if using remote VMs):
```bash
sudo ufw allow ssh
# or
sudo ufw allow 22/tcp
```

List UFW application profiles:
```bash
sudo ufw app list
# Available applications:
#   Apache
#   Apache Full
#   Apache Secure
```

- `Apache` → allows port 80 (HTTP)
- `Apache Full` → allows 80 (HTTP) and 443 (HTTPS)
- `Apache Secure` → allows 443 (HTTPS)

Allow Apache:
```bash
sudo ufw allow 'Apache Full'
```

Enable the firewall:
```bash
sudo ufw enable
# Firewall is active and enabled on system startup
```

Check rules:
```bash
sudo ufw status verbose
```

**Note:** On some hosted VMs, cloud provider firewalls (AWS, Azure, GCP) also need port rules in their console.

---

## 🏗 Creating Virtual Hosts (Multiple Sites on One Server)

### Folder setup
```bash
sudo mkdir -p /var/www/kitpro1
sudo mkdir -p /var/www/kitpro2
sudo chown -R $USER:$USER /var/www/kitpro1
sudo chown -R $USER:$USER /var/www/kitpro2
sudo chmod -R 755 /var/www
```

Create simple HTML files:
```bash
echo "<h1>kitpro1</h1>" > /var/www/kitpro1/index.html
echo "<h1>kitpro2</h1>" > /var/www/kitpro2/index.html
```

### Virtual host config (sites-available)
Copy the default config and edit:
```bash
cd /etc/apache2/sites-available
sudo cp 000-default.conf kitpro1.conf
sudo cp 000-default.conf kitpro2.conf
sudo nano kitpro1.conf
```

Example `kitpro1.conf`:
```apache
<VirtualHost *:80>
    ServerAdmin webmaster@localhost
    ServerName kitpro1.local
    ServerAlias www.kitpro1.local
    DocumentRoot /var/www/kitpro1
    ErrorLog ${APACHE_LOG_DIR}/kitpro1_error.log
    CustomLog ${APACHE_LOG_DIR}/kitpro1_access.log combined
    <Directory /var/www/kitpro1>
        Options Indexes FollowSymLinks
        AllowOverride None
        Require all granted
    </Directory>
</VirtualHost>
```

Repeat for `kitpro2.conf` with `kitpro2.local` and `/var/www/kitpro2`.

Enable sites and reload Apache:
```bash
sudo a2ensite kitpro1.conf
sudo a2ensite kitpro2.conf
sudo apache2ctl configtest   # should output "Syntax OK"
sudo systemctl reload apache2
```

If you want to disable the default site:
```bash
sudo a2dissite 000-default.conf
sudo systemctl reload apache2
```

---

## 🧭 Testing the sites locally — Hosts file

For local testing (on your development PC), map `kitpro1.local` and `kitpro2.local` to `127.0.0.1` using the hosts file.

### Linux (or WSL) hosts file:
Edit `/etc/hosts` (requires sudo):
```bash
sudo nano /etc/hosts
```
Add:
```
127.0.0.1   kitpro1.local kitpro2.local
```

### Windows hosts file (if you're using WSL + browser on Windows)
Edit `C:\Windows\System32\drivers\etc\hosts` with administrator privileges and add the same line.

**Why this works:** The hosts file is checked before DNS. If it contains an entry, your system will use it (127.0.0.1 = localhost), so the browser will hit your local Apache.

---

## 🔒 Permissions & Ownership Recap for Webroot

- Web files should be owned by the user who manages them (or a web user like `www-data`), and readable by Apache.
- Common pattern for development:
```bash
sudo chown -R $USER:$USER /var/www/kitpro1
sudo chmod -R 755 /var/www/kitpro1
```
- For production, you may want files owned by `www-data:www-data` and more restrictive permissions:
```bash
sudo chown -R www-data:www-data /var/www/kitpro1
sudo find /var/www/kitpro1 -type d -exec chmod 755 {} \;
sudo find /var/www/kitpro1 -type f -exec chmod 644 {} \;
```

---

## 🧾 Logs & Troubleshooting

- Apache error log: `/var/log/apache2/error.log`
- Apache access log: `/var/log/apache2/access.log`
- Check latest entries:
```bash
sudo tail -n 50 /var/log/apache2/error.log
sudo tail -n 50 /var/log/apache2/access.log
```

Common debug steps:
1. `sudo apache2ctl configtest` -> fix syntax errors.
2. Check file permissions (Apache must be able to read DocumentRoot).
3. Ensure firewall allows port 80/443.
4. Check `systemctl status apache2` for process errors.
5. Look at logs for details.

---

## 🧠 How the Request Resolution Works (Browser → Hosts/DNS → Apache)

1. You type `kitpro1.local` in the browser.
2. OS checks **hosts file** (`/etc/hosts` or Windows hosts).
3. If not found, it checks local DNS resolver cache.
4. If not found, it queries configured DNS servers (ISP / public DNS).
5. Once IP found, browser sends a request to that IP (port 80 for HTTP).
6. Apache receives request, checks `ServerName` / `ServerAlias` to pick virtual host, serves files from DocumentRoot.

**Hosts file wins over DNS.** Use it for local testing.

---

## ✅ Quick Command Reference

```bash
# Apache
apache2 -v
sudo systemctl status apache2
sudo systemctl restart apache2
sudo apache2ctl configtest

# Networking
ip a
hostname -I
ss -tuln | grep :80
curl -I http://localhost

# UFW
sudo ufw status
sudo ufw allow ssh
sudo ufw allow 'Apache Full'
sudo ufw enable
sudo ufw status verbose

# Virtual hosts
sudo mkdir -p /var/www/kitpro1
sudo chown -R $USER:$USER /var/www/kitpro1
sudo cp /etc/apache2/sites-available/000-default.conf /etc/apache2/sites-available/kitpro1.conf
sudo a2ensite kitpro1.conf
sudo systemctl reload apache2

# Logs
sudo tail -n 50 /var/log/apache2/error.log
```

---

## 🧾 Example session snippet (condensed)

```bash
sudo ufw allow ssh
sudo ufw allow 'Apache Full'
sudo ufw enable

sudo mkdir -p /var/www/kitpro1 /var/www/kitpro2
sudo chown -R $USER:$USER /var/www/kitpro1 /var/www/kitpro2
sudo chmod -R 755 /var/www

sudo cp /etc/apache2/sites-available/000-default.conf /etc/apache2/sites-available/kitpro1.conf
# edit kitpro1.conf -> DocumentRoot /var/www/kitpro1, ServerName kitpro1.local
sudo a2ensite kitpro1.conf
sudo apache2ctl configtest
sudo systemctl reload apache2
```
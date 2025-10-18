
## ⚙️ APT (Advanced Package Tool)

`apt` provides a high-level command-line interface for the package management system.

### 📦 Common Commands

| Command | Description |
|----------|--------------|
| `sudo apt-get install <package>` | Installs a package from the internet repositories |
| `sudo apt-get update` | Updates package lists from all repositories (checks for updates but does not install) |
| `sudo apt-get upgrade` | Upgrades all installed packages to the latest available version |
| `sudo apt-get update && sudo apt-get upgrade -y` | Updates package list and upgrades all packages automatically (with confirmation `-y`) |
| `sudo apt remove <package>` | Removes a package but keeps its configuration files |
| `sudo apt-get purge <package>` | Removes a package along with its configuration files |
| `sudo apt-get --purge autoremove` | Removes unneeded dependencies and config files |
| `sudo apt-cache policy <package>` | Lists all available versions of a package |
| `sudo apt-cache show <package>` | Shows detailed information about a package |
| `sudo apt-get install -f` | Fixes broken dependencies |

---

## 📚 Concepts

### 🧩 What is a Package?

A **package** is a group of files that make up a software application.

### 🔗 Dependencies

A **dependency** means a package requires other packages to work correctly. Linux package management systems automatically handle dependencies for you.

### 🏦 Repository

A **repository** is a digital library for storing software packages. When installing a program, APT searches these repositories and fetches all required files and dependencies.

---

## 🧱 dpkg (Debian Package Manager)

`dpkg` is a lower-level tool used to install, query, and remove `.deb` packages manually. It does **not** automatically handle dependencies.

### 🧰 Common Commands

| Command | Description |
|----------|--------------|
| `dpkg -l` | Lists all installed packages |
| `dpkg -p <package>` | Displays detailed information about a package |
| `dpkg -s <package>` | Checks whether a package is installed or not |
| `dpkg --print-architecture` | Displays OS architecture (e.g., amd64) |
| `sudo dpkg -i <package.deb>` | Installs a `.deb` package manually |
| `sudo dpkg -r <package>` | Removes a package (keeps config files) |
| `sudo dpkg -P <package>` | Removes package along with config files |

---

## 🌐 Downloading Packages Manually

Use `wget` to download `.deb` packages from the internet.

```bash
wget http://mirrors.kernel.org/ubuntu/pool/universe/f/fping/fping_5.1-1_amd64.deb
sudo dpkg -i fping_5.1-1_amd64.deb
```

> ⚠️ If dependencies are missing, `dpkg` will fail.  
Use `sudo apt-get install -f` to automatically resolve and install the missing dependencies.

---

## 🏗 Why APT is Better Than dpkg

While `dpkg` installs `.deb` files manually, it does **not** handle dependencies.  
APT is a **smart wrapper** around `dpkg` that automatically resolves and installs all required dependencies.

---

## 🧠 Update vs Upgrade

| Command | Function |
|----------|-----------|
| `sudo apt-get update` | Fetches updated package lists from repositories. Think of it as “Check for Updates.” |
| `sudo apt-get upgrade` | Installs new versions of installed packages based on the updated lists. |
| ✅ **Always run:** `sudo apt-get update` **before** `sudo apt-get upgrade`. |

---

## 👤 User Management Commands

| Command | Description |
|----------|--------------|
| `sudo adduser <username>` | Adds a new user and creates a home directory |
| `su <username>` | Switches to another user |
| `exit` | Returns to the previous user |
| `sudo userdel <username>` | Deletes a user (fails if user is logged in) |
| `sudo userdel -f -r <username>` | Forcibly removes user and their home directory |

---

## 👥 Group Management Commands

| Command | Description |
|----------|--------------|
| `sudo groupadd <groupname>` | Creates a new group |
| `sudo groupdel <groupname>` | Deletes a group |
| `sudo gpasswd -a <user> <group>` | Adds user to a group |
| `sudo gpasswd -M <user1,user2,...> <group>` | Defines multiple users as members of a group |
| `sudo chgrp <group> <file>` | Changes the group ownership of a file |

### Example

```bash
sudo groupadd devops
sudo groupadd tester
sudo gpasswd -a ronaldo devops
sudo gpasswd -a izhan devops
sudo gpasswd -M messi,kaka,zidane tester
sudo chgrp devops notes.txt
```

---

## 🧩 Summary Table

| Tool | Purpose |
|------|----------|
| `apt` | High-level package management (handles dependencies) |
| `dpkg` | Low-level tool for manual `.deb` package installation |
| `wget` | Downloads files from the internet |
| `adduser`, `userdel`, `groupadd`, `gpasswd` | Manage users and groups |

---

## 🧠 Quick Recap

- **Always run** `sudo apt-get update` before installing/upgrading packages.  
- Use **APT** instead of **dpkg** for dependency handling.  
- Use `apt-cache show` or `policy` to inspect packages before installation.  
- Combine commands smartly:  
  ```bash
  sudo apt-get update && sudo apt-get upgrade -y
  ```  
- **Users and Groups** are essential for permission control in Linux.


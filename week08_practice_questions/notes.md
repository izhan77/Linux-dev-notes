# 🖥️ Operating Systems — Bash & Linux Practice (Exam Prep)

---

## SECTION A — Linux Directories & File Management (Q & Corrected Answers)

**Tasks** and **recommended commands** with short explanations and exam tips.

1. **Create a directory named `practice1` inside your home directory.**  
```bash
mkdir ~/practice1
```

2. **Create two subdirectories `docs` and `images` inside `practice1`.**  
```bash
mkdir -p ~/practice1/docs ~/practice1/images
```

3. **Create a text file `readme.txt` inside `docs` containing your name.**  
```bash
echo "Muhammad Izhan Waheed" > ~/practice1/docs/readme.txt
```

4. **Copy `readme.txt` to `/tmp` and rename it `info.txt`.**  
```bash
cp ~/practice1/docs/readme.txt /tmp/info.txt
```

5. **Move the file `info.txt` from `/tmp` to `/var/www/`.**  
```bash
sudo mv /tmp/info.txt /var/www/
```

> ✳️ Note: `/var/www` typically requires `sudo` for write operations.

6. **Create a hidden file `.config` inside your home directory.**  
```bash
touch ~/.config
```

7. **Display only directories from your current location.**  
```bash
ls -d */
```

8. **Display all hidden files and directories.**  
```bash
ls -la
# or
ls -d .?*  # lists hidden entries (be careful with . ..)
```

9. **Delete a file named `delete_me.txt` from your home directory.**  
```bash
rm ~/delete_me.txt
```

10. **Copy all `.txt` files from one directory to another using a single command.**  
```bash
cp ~/source_dir/*.txt ~/dest_dir/
```

11. **Create a directory `nested/test1/test2/test3` in one command.**  
```bash
mkdir -p nested/test1/test2/test3
```

12. **Display your current working directory path.**  
```bash
pwd
```

13. **Count the number of files in your home directory.**  
```bash
find ~ -type f | wc -l
```

14. **Rename a file `notes.txt` to `final_notes.txt`.**  
```bash
mv notes.txt final_notes.txt
```

15. **Combine contents of `a.txt` and `b.txt` into a new file `combined.txt`.**  
```bash
cat a.txt b.txt > combined.txt
```

16. **Create a symbolic link of `/var/www/html` inside your home directory.**  
```bash
ln -s /var/www/html ~/html
```

> 🔁 A symbolic link (`ln -s`) creates a shortcut. `cd ~/html` will take you to `/var/www/html`.

17. **List all files in `/etc` that were modified in the last 24 hours.**  
```bash
find /etc -type f -mtime -1
```

**What `-mtime` means:** `-mtime -1` = modified within the last 24 hours. `-mtime 1` = 24–48 hours ago. `-mtime +1` = modified more than 24-48 hours

18. **Compress a folder `project` into `project.tar.gz`.**  
```bash
tar -czf project.tar.gz project/
```

19. **Extract `project.tar.gz` back into a folder named `project_copy`.**  
```bash
mkdir -p project_copy
tar -xzf project.tar.gz -C project_copy/
```

20. **Find and display the size of all `.log` files inside `/var/log`.**  
```bash
sudo find /var/log -type f -name "*.log" -exec ls -lh {} \;
```

> 🔎 Tip: `-exec ls -lh {} \;` shows human-readable sizes; `sudo` may be required for some log files.

---

## SECTION B — Permissions & Ownership (Q & Corrected Answers + Explanations)

1. **Set read, write, execute permissions for the owner only on `file1.txt`.**  
```bash
chmod 700 file1.txt   # owner=rwx, group=---, others=---
```

2. **Give read and execute permission to everyone for `run.sh`.**  
```bash
chmod a+rx run.sh     # equivalent to chmod 755 run.sh (owner keep write)
# or explicitly:
chmod 755 run.sh
```

3. **Remove all permissions from `temp.txt` for others (i.e., 'others' should have no permissions).**  
```bash
chmod o= temp.txt     # clears permissions for 'others'
# or set numeric so others=0:
chmod 640 temp.txt    # owner=rw, group=r, others=0
```

4. **Set numeric permission 640 on `data.csv`.**  
```bash
chmod 640 data.csv    # owner=rw, group=r, others=0
```

5. **Make `script.sh` executable for the owner and group only.**  
```bash
chmod 750 script.sh   # owner=rwx, group=rx, others=0
```

6. **Create a new user `testuser` and assign ownership of `testfile.txt` to them.**  
```bash
sudo adduser testuser
sudo chown testuser:testuser testfile.txt
```

7. **Change the group of `file.txt` to `www-data`.**  
```bash
sudo chgrp www-data file.txt
# or
sudo chown :www-data file.txt
```

8. **Display file permissions of all files in the current directory.**  
```bash
ls -l
```

9. **Add write permission for the group to `file.txt`.**  
```bash
chmod g+w file.txt
```

10. **Copy file permissions from `source.txt` to `target.txt`.**  
```bash
chmod --reference=source.txt target.txt
```

11. **Set permissions on all `.sh` files so that only the owner can execute them.**  
```bash
# Option 1: only owner can execute, group/others cannot:
chmod 700 *.sh

# Option 2: owner execute + read allowed for others (less strict):
chmod 744 *.sh
```

12. **Recursively set read-only permission for everyone inside `/home/student/docs`.**  
Files and directories need slightly different bits: directories must be searchable/executable to `cd`. A safe approach:

```bash
# Make directories readable+executable (so they can be entered), files readable:
find /home/student/docs -type d -exec chmod 555 {} \;
find /home/student/docs -type f -exec chmod 444 {} \;
```

13. **View all files owned by the current user.**  
```bash
find / -user $USER -type f 2>/dev/null
# Or limit to home:
find ~ -user $USER -type f
```

> ⚠️ **Caution:** `find /` is expansive and may be slow. Use `sudo` if needed or constrain the path.

---

## SECTION C — Apache Service & Configuration (Q & Corrected Answers + Notes)

1. **Check if the Apache service is running.**  
```bash
sudo systemctl status apache2
```

2. **Start the Apache service if it is stopped.**  
```bash
sudo systemctl start apache2
```

3. **Stop the Apache service and verify.**  
```bash
sudo systemctl stop apache2
sudo systemctl status apache2
```

4. **Restart the Apache service.**  
```bash
sudo systemctl restart apache2
```

5. **Reload the Apache configuration without restarting (graceful).**  
```bash
sudo systemctl reload apache2
```

6. **View the status of Apache and redirect it to `apache_status.txt`.**  
```bash
sudo systemctl status apache2 &> apache_status.txt
```

7. **Display Apache version installed.**  
```bash
apache2 -v
# or
apachectl -v
```

8. **Create a directory `/var/www/practice_site` and add an `index.html` page.**  
```bash
sudo mkdir -p /var/www/practice_site
sudo tee /var/www/practice_site/index.html > /dev/null <<'HTML'
<html>
<head><title>OS Lab Practice</title></head>
<body>
<h1>Hello from Linux!</h1>
<p>This is my webpage deployed using Apache :)</p>
</body>
</html>
HTML
```

9. **Modify the document root in the default Apache config to `/var/www/practice_site`.**  
Edit `/etc/apache2/sites-available/000-default.conf` and change `DocumentRoot /var/www/html` to `DocumentRoot /var/www/practice_site`. Then save. Example:

```bash
sudo sed -i 's#/var/www/html#/var/www/practice_site#g' /etc/apache2/sites-available/000-default.conf
sudo systemctl reload apache2
```

10. **Enable the site and restart Apache.**  
If you created a new vhost file `/etc/apache2/sites-available/practice.conf`:
```bash
sudo a2ensite practice.conf
sudo systemctl restart apache2
```

11. **Disable the default site and re-enable it.**  
```bash
sudo a2dissite 000-default.conf
sudo systemctl reload apache2

# Re-enable:
sudo a2ensite 000-default.conf
sudo systemctl reload apache2
```

12. **Create an alias `/docs` pointing to `/var/www/html`.**

> There are two possible interpretations:
> - **Apache URL alias** (so a web URL `http://server/docs` serves files from `/var/www/html`), or
> - **Shell alias** (a shortcut command).

**Apache Alias (in a vhost or a2conf file):**
```apacheconf
Alias /docs /var/www/html
<Directory /var/www/html>
    Require all granted
</Directory>
```

Then `sudo systemctl reload apache2`.

**Shell alias (local command):**
```bash
alias docs='cd /var/www/html'
```

13. **Check syntax errors in Apache configuration.**  
```bash
sudo apache2ctl configtest
# or
sudo apachectl -t
```

14. **View the list of all enabled Apache modules.**  
```bash
apache2ctl -M
```

**Network/ports tips:**  
- Install `net-tools` if you prefer `netstat`: `sudo apt install net-tools`  
- Check ports Apache listens on:
```bash
sudo ss -tulpn | grep -E 'apache|httpd|:80|:443'
sudo grep -R "Listen" /etc/apache2/
```

---

## SECTION D — Bash Scripting & Cron Automation (20 Questions — Answers, Notes & Fixes)

1. **Create a bash script that prints “Hello OS Lab”.**  
```bash
#!/bin/bash
echo "Hello OS Lab"
```

2. **Write a script that displays the current date and time.**  
```bash
#!/bin/bash
echo "Date : $(date +'%d-%m-%Y')"
echo "Time : $(date +'%H:%M:%S')"
```

3. **Write a script that lists all files in `/home/student`.**  
```bash
ls -la /home/student
```

4. **Write a script that displays the number of files in the current directory.**  
```bash
count=$(find . -maxdepth 1 -type f | wc -l)
echo "Number of files in current directory: $count"
```

5. **Create a script that backs up `/etc` into `/home/student/etc_backup.tar.gz`.**  
```bash
#!/bin/bash
sudo tar -czf /home/student/etc_backup.tar.gz /etc
```

6. **Add a cron job to run the above backup script daily at 2:00 AM.**  
```cron
0 2 * * * /home/student/backup_etc.sh
```

7. **Add a cron job to run every 5 minutes.**  
```cron
*/5 * * * * /path/to/script.sh
```

8. **Add a cron job to execute every Monday at 9:30 AM.**  
```cron
30 9 * * 1 /path/to/script.sh
```

9. **Add a cron job to run a script at system startup.**  
```cron
@reboot /home/student/start_services.sh
```

10. **Display the list of current user's cron jobs.**  
```bash
crontab -l
# or for another user:
sudo crontab -u username -l
```

11. **Remove all cron jobs for the current user.**  
```bash
crontab -r
```

12. **Redirect both standard output and errors of a script to a log file.**  
```bash
./script.sh &> /home/student/script_test.log
# or
./script.sh > /home/student/script_test.log 2>&1
```

13. **Write a script that checks if Apache is running; if not, start it.**  
```bash
#!/bin/bash
if ! systemctl is-active --quiet apache2; then
  sudo systemctl start apache2
  echo "Apache was stopped — started it."
else
  echo "Apache is running."
fi
```

14. **Schedule a script to clear `/tmp` every 12 hours.**  
Crontab entry (runs at 00:00 and 12:00 every day):
```cron
0 */12 * * * /home/student/clear_tmp.sh
```

15. **Create a script that appends the date to `/home/student/log.txt` every run.**  
```bash
#!/bin/bash
echo "$(date '+%Y-%m-%d %H:%M:%S') - script ran" >> /home/student/log.txt
```

16. **Make a script executable and verify using `ls -l`.**  
```bash
chmod +x /home/student/myscript.sh
ls -l /home/student/myscript.sh
```

17. **Write a script that pings google.com and logs the result.**  
```bash
#!/bin/bash
LOG_FILE="/home/student/ping.logs"
echo "$(date +'%Y-%m-%d %H:%M:%S') - Pinging google.com..." >> "$LOG_FILE"
ping -c 4 google.com >> "$LOG_FILE" 2>&1
echo "-----------------------------------------" >> "$LOG_FILE"
```

18. **Schedule a cron job that executes every 15 minutes between 9 AM and 5 PM.**  
```cron
*/15 9-17 * * * /path/to/script.sh
```

19. **Write a script that prints “Backup started” → sleeps 5 seconds → “Backup complete”.**  
```bash
#!/bin/bash
echo "Backup started"
sleep 5
echo "Backup complete"
```

20. **Add a cron job that runs the above script every day at 11:59 PM.**  
```cron
59 23 * * * /home/student/backup_sim.sh
```

---

## ✅ Common Exam Pitfalls & Quick Tips

- **Shebang matters.** `./script.sh` uses the shebang to pick interpreter. `bash script.sh` ignores shebang and runs with Bash explicitly.  
- **`expr` requires spaces.** `expr 30 + 10` not `expr 30+10`. For arithmetic, prefer `$(( ))`.  
- **Use `-p` with `mkdir`** to avoid errors when creating nested directories.  
- **When changing permissions recursively** be careful — don't `chmod -R 777 /` (terrifying). Always test on a sample folder.  
- **Cron timezone** uses system timezone. If jobs look off, check `/etc/timezone`.  
- **For directories to be enterable** they need the execute (`x`) bit. Setting `444` on directories makes them non-enterable; use `555` if you want read-only + enterable.  
- **Always use absolute paths in cron scripts.** Cron runs with a limited PATH. Use full paths for commands and files.  
- **Check logs** `/var/log/syslog`, `/var/log/cron` or your script log to debug cron failures.



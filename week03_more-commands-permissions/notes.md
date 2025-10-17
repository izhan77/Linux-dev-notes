
## 📊 Permission Basics (Owner / Group / Others)

Every file and directory in Linux has **three categories of users** and **three permission types**:

- **User categories**
  - **Owner (u)** — the user who owns the file
  - **Group (g)** — users in the file's group
  - **Others (o)** — everyone else

- **Permission types**
  - **Read (r)** — view file contents / list directory
  - **Write (w)** — modify/delete file / create/delete entries in directory
  - **Execute (x)** — run file as program / enter directory and access its contents

---

## 🧮 Numeric (Octal) Permission System

Each permission is one bit. The decimal mapping:

| Permission | Binary | Decimal |
|------------|--------:|--------:|
| Read (r)   | 100    | 4 |
| Write (w)  | 010    | 2 |
| Execute (x)| 001    | 1 |

Combine bits by summing:

- `0` = `---`
- `1` = `--x`
- `2` = `-w-`
- `3` = `-wx`
- `4` = `r--`
- `5` = `r-x`
- `6` = `rw-`
- `7` = `rwx`

Example: `chmod 754 file.txt` → owner `7` (rwx), group `5` (r-x), others `4` (r--).  
Result shown by `ls -l`:
```
-rwxr-xr--
```

---

## 🔍 How Permissions Behave

### Read (r = 4)
- **Files:** can view contents (e.g., `cat`, `less`).
- **Dirs:** can list filenames (`ls`), but cannot read file contents inside unless `x` is also set on the directory.

### Write (w = 2)
- **Files:** can modify, truncate, or overwrite file.
- **Dirs:** can create or delete entries inside the directory (delete requires write on the directory, not necessarily write on the file).

### Execute (x = 1)
- **Files:** required to run a script or binary (`./script.sh`).
- **Dirs:** required to `cd` into the directory and access inodes inside (even if you can `ls` the directory, you may not access files without `x`).

---

## 🗂 Directory Examples

```bash
# Public readable and executable (owner can write)
mkdir mydir
chmod 755 mydir
ls -ld mydir    # drwxr-xr-x

# Private directory (only owner)
mkdir private
chmod 700 private  # drwx------
ls -ld private
```

**Explanation:** 755 means `rwx` for owner, `r-x` for group, `r-x` for others. Group/others can list & enter but not modify.

---

## ⚙️ Common Permission Patterns

| Use case | Numeric | Symbolic | Meaning |
|---------:|--------:|---------:|--------|
| Executable script | `755` | `rwxr-xr-x` | Owner can edit, everyone can run |
| Config file | `644` | `rw-r--r--` | Owner read/write, others read only |
| Private file | `600` | `rw-------` | Only owner can read/write |
| Shared group file | `664` | `rw-rw-r--` | Owner & group can edit |
| Private dir | `700` | `rwx------` | Only owner can use dir |
| Shared dir | `775` | `rwxrwxr-x` | Owner & group full, others read/exec |

---

## 🔧 Quick Commands (Permissions & Ownership)

```bash
# View permissions
ls -l filename
ls -ld directoryname

# Change mode numerically
chmod 755 script.sh
chmod 644 file.txt

# Change owner and group
chown newowner filename
chown newowner:newgroup filename

# Recursive operations
chmod -R 755 somedir
chown -R www-data:www-data /var/www/html
```

**Notes:**
- `chown` usually requires `sudo` for non-owning users.
- `chmod -R` will change everything recursively; be careful in system paths.

---

## 📌 Practical Examples (Fixed & Clarified)

### Example: `chmod 754 file.txt`
- Owner `7` = `rwx`
- Group `5` = `r-x`
- Others `4` = `r--`
```
-rwxr-xr--
```

### Example: `chmod 640 secret.txt`
- Owner `6` = `rw-`
- Group `4` = `r--`
- Others `0` = `---`
```
-rw-r-----
```

### Script file example
```bash
echo 'echo "Hello World"' > myscript.sh
chmod 755 myscript.sh
./myscript.sh
```

### Config file (read-only)
```bash
chmod 644 config.conf  # -rw-r--r--
```

---

## 🛠 Ownership in practice

Example session (corrected explanation):

```bash
# Initial listing
ls -l
# -rw-r--r-- 1 izhan izhan 0 Oct 16 10:38 file.txt
# --w------- 1 izhan izhan 3 Oct 16 10:41 test.txt

# Attempt to change owner (fails without sudo)
chown root test.txt
# chown: changing ownership of 'test.txt': Operation not permitted

# Use sudo to change owner
sudo chown root test.txt
ls -l
# -rw-r--r-- 1 izhan izhan 0 Oct 16 10:38 file.txt
# --w------- 1 root  izhan 3 Oct 16 10:41 test.txt
```

**Explanation:** Only root (or sudo) can change file ownership. Group remains as shown (`izhan` in example).

---

## 🔎 `grep` — Search Inside Files (fixed examples & flags)

`grep` searches text using patterns (regular expressions). Useful options:

- `-i` — case-insensitive  
- `-w` — match whole words  
- `-n` — show line numbers  
- `-r` — recursive search in directories

Examples:
```bash
grep "Hulk" names.txt            # exact case match
grep -i "izhan" names.txt        # case-insensitive
grep -w "Izhan" names.txt        # whole-word match
grep -n "Izhan" names.txt        # show line numbers
grep -ni "Ronaldo" names.txt     # case-insensitive with numbers
grep -B 3 "Ronaldo" names.txt    # show 3 lines before match
grep -r "TODO" ~/projects        # search recursively
```

---

## 🖥 System Info & Useful Commands

```bash
uname           # kernel name (e.g., Linux)
uname -a        # full kernel info
uname -r        # kernel release
uname -m        # machine hardware (x86_64)
uname -o        # operating system (GNU/Linux)

hostname        # show the hostname
id              # show current user id and groups

top             # interactive process viewer (press q to quit)
htop            # better visual process viewer (install if missing)
```

---

## ✅ Corrected Examples from Your Session

- `sudo useradd User` adds a user but you should set a password with `sudo passwd User`.
- `passwd User` without `sudo` may fail because only privileged users can change another user's password.
- `uname` returns `Linux`; `uname -a` gives more detail; `hostname` returns the machine name.

---

## 🧠 Short Quizzes (Self-check)

1. What numeric mode sets `rwx` for owner, `r-x` for group, and no access for others?  
2. How do you make a script executable only by its owner?  
3. If a directory is `drwxr-x---`, who can `cd` into it?  
4. How do you change both owner and group of a file in one command?  
5. Why does `sudo chown root file.txt` succeed where `chown root file.txt` fails?

(Answers at the bottom of the file.)

---

## 🧾 Quick Reference Commands — Summary

```bash
ls -l
ls -ld directoryname
chmod 755 file.sh
chmod u+x script.sh
chmod -R 755 somedir
chown user:group file
chown -R user:group somedir
sudo chown root file.txt
grep -ri "pattern" path/
uname -a
hostname
id
top
```

---

## 📝 Quiz Answers

1. `740`?  **No** — correct is `750` or `740`?  
   - The question: `rwx` for owner (7), `r-x` for group (5), no access for others (0) → **`750`**.
2. `chmod 700 script.sh` or `chmod u+x script.sh` and remove others (`chmod 700`).
3. `drwxr-x---` → owner and group members (because group has `r-x` which includes execute) can `cd`. Others cannot.
4. `chown newowner:newgroup filename`
5. Because `chown` requires superuser privileges to change ownership — `sudo` elevates privileges.


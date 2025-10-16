
## 💻 Terminal Prompt Breakdown

Example:
```bash
izhan@Izhan7:~$
```

- **izhan** → Username of the current logged-in user  
- **@** → Separator between username and hostname  
- **Izhan7** → Hostname of the system (device/server name)  
- **~** → Represents the **home directory**  

---

## 📁 File and Directory Commands

| Command | Description | Example |
|----------|--------------|----------|
| `ls` | Lists all files and directories | `ls` |
| `ls -a` | Lists **all files** including hidden ones | `ls -a` |
| `ls -l` | Lists files in **detailed (long)** format | `ls -l` |
| `ls -R` | Lists files **recursively** (including subdirectories) | `ls -R` |
| `pwd` | Prints the **current working directory** | `pwd` |
| `mkdir folderName` | Creates a new directory | `mkdir test` |
| `mkdir -p random/middle/hello` | Creates **nested directories** if parents don’t exist | `mkdir -p random/middle/hello` |
| `cd folderName` | Changes current directory | `cd Documents` |
| `cd ..` | Moves **one level up** | `cd ..` |
| `cd ~` | Returns to **home directory** | `cd ~` |

---

## 📄 File Management

| Command | Description | Example |
|----------|--------------|----------|
| `touch file.txt` | Creates a **new empty file** | `touch note.txt` |
| `cat file.txt` | Displays the **contents** of a file | `cat file.txt` |
| `cat > file.txt` | Creates file and allows writing (overwrites if exists) | `cat > file.txt` |
| `echo "Hello World"` | Prints a message to terminal | `echo "Hello World"` |
| `echo "Hello" > file.txt` | Writes message into a file (overwrites) | — |
| `echo "New line" >> file.txt` | **Appends** message to file | — |
| `cp file1.txt file2.txt` | Copies a file | `cp file1.txt copy.txt` |
| `mv file1.txt newName.txt` | Moves or **renames** a file | — |
| `rm file.txt` | Deletes file **permanently** | `rm test.txt` |
| `nano file.txt` | Opens file in **Nano text editor** | — |

---

## 🧰 File Viewing & Comparison

| Command | Description | Example |
|----------|--------------|----------|
| `head file.txt` | Shows first 10 lines | `head file.txt` |
| `head -n 4 file.txt` | Shows first 4 lines | — |
| `tail file.txt` | Shows last 10 lines | — |
| `tail -n 4 file.txt` | Shows last 4 lines | — |
| `diff file1.txt file2.txt` | Compares two files line-by-line | — |
| `cat file.txt | tr a-z A-Z > upper.txt` | Converts lowercase → uppercase and saves output | — |

---

## 🔍 Search and Find

| Command | Description | Example |
|----------|--------------|----------|
| `find . -type d` | Finds **directories** in current folder | — |
| `find . -type f` | Finds **files** in current folder | — |
| `find . -type f -name "*.txt"` | Finds all **.txt** files recursively | — |

---

## ⚙️ System Info and Utilities

| Command | Description | Example |
|----------|--------------|----------|
| `df` | Reports **disk space usage** | — |
| `du` | Estimates **file/folder size** | — |
| `sudo command` | Executes a command with **admin privileges** | `sudo apt update` |
| `man command` | Opens **manual/help** for any command | `man ls` |
| `\` | Continues a command on the **next line** | `cat file.txt \` |

---

## 🧩 Example: Creating and Manipulating Files

```bash
# Step 1: Create a new directory
mkdir -p project/demo

# Step 2: Go inside it
cd project/demo

# Step 3: Create two files
touch file1.txt file2.txt

# Step 4: Add text into file1
echo "Linux is powerful" > file1.txt

# Step 5: Convert file1 text to uppercase and save to new file
cat file1.txt | tr a-z A-Z > upper.txt

# Step 6: Compare files
diff file1.txt upper.txt

# Step 7: Check current location
pwd
```

---

## 🏁 Final Tips

- Always use `man <command>` when you’re unsure — it’s your best built-in help.  
- Be **careful** with commands like `rm` — they delete files permanently.  
- Combine commands using `|` (pipe) and `>` (redirect) to automate workflows.  



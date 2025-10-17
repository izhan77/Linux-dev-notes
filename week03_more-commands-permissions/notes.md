📊 The Basic Concept
Every file and directory in Linux has 3 types of permissions for 3 categories of users:

👥 User Categories:
Owner (u) - The user who owns the file

Group (g) - Users who are in the file's group

Others (o) - Everyone else on the system

🔑 Permission Types:
Read (r) - Permission to view/file contents

Write (w) - Permission to modify/delete

Execute (x) - Permission to run/access

🧮 The Number System Explained
Binary Foundation:
Each permission is represented by 1 bit (on/off):

Permission	Binary Value	Decimal Value
Read (r)	100	4
Write (w)	010	2
Execute (x)	001	1
🎯 How Combinations Work:
Permission Combination	Binary	Calculation	Decimal
No permissions	000	0	0
Execute only	001	1	1
Write only	010	2	2
Write + Execute	011	2+1=3	3
Read only	100	4	4
Read + Execute	101	4+1=5	5
Read + Write	110	4+2=6	6
Read + Write + Execute	111	4+2+1=7	7
🔍 Deep Dive into Each Permission
📖 READ Permission (r = 4)
For Files:

Can view/open the file content

Can copy the file

Can read metadata (size, dates)

For Directories:

Can list directory contents with ls

Cannot access files inside without execute permission

Example:

bash
chmod 400 file.txt    # Owner can only read
cat file.txt          # ✓ Works
echo "hi" > file.txt  # ✗ Fails (no write)
✏️ WRITE Permission (w = 2)
For Files:

Can modify file content

Can delete the file

Can truncate/append to file

For Directories:

Can create new files/directories inside

Can delete files inside (even without file write permission!)

Can rename files inside

Example:

bash
chmod 200 file.txt    # Owner can only write
echo "hi" > file.txt  # ✓ Works (but creates new content)
cat file.txt          # ✗ Fails (no read)
🚀 EXECUTE Permission (x = 1)
For Files:

Can run the file as a program/script

Required for binaries (.exe, compiled programs)

Required for shell scripts (.sh)

For Directories:

Can cd into the directory

Can access files/subdirectories inside

Required to read file contents in the directory

Example:

bash
chmod 100 script.sh   # Owner can only execute
./script.sh           # ✓ Works if it's executable
cat script.sh         # ✗ Fails (no read)
🧩 Practical Examples Breakdown
Example 1: chmod 754 file.txt
text
7   5   4
↓   ↓   ↓
u   g   o
Calculation:

User: 7 = 4(r) + 2(w) + 1(x) = rwx

Group: 5 = 4(r) + 0(w) + 1(x) = r-x

Others: 4 = 4(r) + 0(w) + 0(x) = r--

Visual:

text
-rwxr-xr--
↑└┬┘└┬┘└┬┘
 │  │  │ Others: r--
 │  │  Group: r-x
 │  Owner: rwx
 File type (- = regular file)
Example 2: chmod 640 secret.txt
text
6   4   0
↓   ↓   ↓
u   g   o
Calculation:

User: 6 = 4(r) + 2(w) + 0(x) = rw-

Group: 4 = 4(r) + 0(w) + 0(x) = r--

Others: 0 = 0(r) + 0(w) + 0(x) = ---

Result: -rw-r-----

🗂️ Directory Permission Examples
Directory with 755 permissions:
bash
mkdir mydir
chmod 755 mydir
ls -ld mydir    # drwxr-xr-x
What this allows:

Owner: List, create, delete, access files

Group: List and access files, but cannot create/delete

Others: List and access files, but cannot create/delete

Directory with 700 (private):
bash
chmod 700 mydir  # drwx------
Only the owner can do anything with this directory.

🧠 Advanced Scenarios
Script File:
bash
# Create a script
echo 'echo "Hello World"' > myscript.sh
chmod 755 myscript.sh  # -rwxr-xr-x

# Now everyone can execute it
./myscript.sh  # ✓ Works for all users
Config File (read-only):
bash
chmod 644 config.conf  # -rw-r--r--
# Owner can read/write, others can only read
Private Data:
bash
chmod 600 private.txt  # -rw-------
# Only owner can read/write, nobody else can even see content
🛠️ Common Permission Patterns
Use Case	Numeric	Symbolic	Description
Executable script	755	rwxr-xr-x	Everyone can execute
Config file	644	rw-r--r--	Readable by all, writable by owner
Private file	600	rw-------	Only owner can read/write
Shared group file	664	rw-rw-r--	Group members can edit
Private directory	700	rwx------	Only owner has full access
Shared directory	755	rwxr-xr-x	Everyone can list/access
🔧 Quick Reference Commands
bash
# View permissions in detail
ls -l filename
ls -ld directoryname

# Change permissions numerically
chmod 755 file.sh
chmod 644 file.txt
chmod 700 private.txt

# Change permissions symbolically
chmod u+x script.sh      # Add execute for owner
chmod g-w file.txt       # Remove write from group  
chmod o-r secret.txt     # Remove read from others
chmod a+r public.txt     # Add read for everyone (a=all)


chmod - change file mode bits
chown - change file owner and group
chown NEW_OWNER filename
chown NEW_OWNER:NEW_GROUP filename

izhan@Izhan7:~$ ls -l
total 4
-rw-r--r-- 1 izhan izhan 0 Oct 16 10:38 file.txt
--w------- 1 izhan izhan 3 Oct 16 10:41 test.txt
izhan@Izhan7:~$ chown root test.txt
chown: changing ownership of 'test.txt': Operation not permitted
izhan@Izhan7:~$ sudo chown root test.txt
[sudo] password for izhan:
izhan@Izhan7:~$ ls -l
total 4
-rw-r--r-- 1 izhan izhan 0 Oct 16 10:38 file.txt
--w------- 1 root  izhan 3 Oct 16 10:41 test.txt
izhan@Izhan7:~$
  
grep - global regular expression print , it allows us to search text within the file, case sensitive

izhan@Izhan7:~$ grep "Hulk" names.txt
Hulk
izhan@Izhan7:~$ grep "Waheed" names.txt
izhan@Izhan7:~$ grep -w "Izhan" names.txt
Muhammad Izhan Waheed
izhan@Izhan7:~$ grep "izhan" names.txt
izhan@Izhan7:~$ grep -i "izhan" names.txt
Muhammad Izhan Waheed
izhan@Izhan7:~$

izhan@Izhan7:~$ grep -w "Izhan" names.txt
Muhammad Izhan Waheed
izhan@Izhan7:~$ grep "izhan" names.txt
izhan@Izhan7:~$ grep -i "izhan" names.txt
Muhammad Izhan Waheed
izhan@Izhan7:~$ grep -n "Izhan" names.txt
1:Muhammad Izhan Waheed
izhan@Izhan7:~$ grep -n "Ronaldo" names.txt
7:Ronaldo Nazario
izhan@Izhan7:~$ grep -ni "Ronaldo" names.txt
2:ROnaldo
7:Ronaldo Nazario
izhan@Izhan7:~$ grep -B 3 "Ronaldo" names.txt
Neymar
Hulk
Roberto
Ronaldo Nazario
izhan@Izhan7:~$

izhan@Izhan7:~$ uname
Linux
izhan@Izhan7:~$ hostname
Izhan7

izhan@Izhan7:~$  top

izhan@Izhan7:~$ uname
Linux
izhan@Izhan7:~$ hostname
Izhan7
izhan@Izhan7:~$ cd ..
izhan@Izhan7:/home$ cd ..
izhan@Izhan7:/$ sudo useradd User
[sudo] password for izhan:
izhan@Izhan7:/$ passwd User
passwd: You may not view or modify password information for User.
izhan@Izhan7:/$ sudo passwd User
New password:
Retype new password:
passwd: password updated successfully

izhan@Izhan7:/$ uname
Linux
izhan@Izhan7:/$ uname -o
GNU/Linux
izhan@Izhan7:/$ uname -m
x86_64

izhan@Izhan7:/$ id
uid=1000(izhan) gid=1000(izhan) groups=1000(izhan),4(adm),24(cdrom),27(sudo),30(dip),46(plugdev),100(users)


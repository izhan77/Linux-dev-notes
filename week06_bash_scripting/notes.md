# 🐧 Bash Scripting - Till Loops

## ⚙️ 1. What Is Shell & How It Works

The **Shell** is a **command-line interpreter** that acts as a **bridge between the user and the Linux kernel**.

When you type a command:
1. Shell takes your input.
2. Sends it to the **kernel**.
3. Kernel interacts with the **hardware**.
4. Result/output is sent back through the shell to your screen.

In short:  
`User → Shell → Kernel → Hardware → Shell → User`

### 🧠 Why Bash?
**Bash (Bourne Again Shell)** is the most commonly used Linux shell.  
It supports command execution, scripting, automation, and system control.

## ⚙️ What is Bash?

**Bash (Bourne Again Shell)** is a command-line shell and scripting language for Unix-based systems.

### 🔹 Key Characteristics
✅ Default shell for most Linux distributions and macOS  
✅ Command interpreter that executes commands  
✅ Scripting language for automation  
✅ Successor to the original Bourne shell (`sh`)

---

## 🧠 Check Your Shell

```bash
# Check your current shell
echo $SHELL
# Output: /bin/bash

# Check shell version
bash --version
```

---

## 📜 Bash Script Fundamentals

### ✨ Creating Your First Script

```bash
#!/bin/bash
# This is a comment
echo "Hello, World!"
```

### 🧩 The Shebang (#!)
The **shebang** tells the system which interpreter to use for the script.

**Key Points:**
- Must be the first line of the script
- Format: `#!/path/to/interpreter`

Examples:
```bash
#!/bin/bash           # Most common
#!/usr/bin/env bash   # More portable (recommended)
```

### 🔐 Making Scripts Executable
```bash
# Create script
nano myscript.sh

# Make executable
chmod +x myscript.sh

# Run script
./myscript.sh
```

### 🚀 Running Scripts: Two Methods
```bash
./script.sh      # Requires executable permission
bash script.sh   # Doesn’t require executable permission
```

---

## 🧱 Variables Deep Dive

### 🔹 Declaration & Usage
```bash
#!/bin/bash
name="John"
age=25
salary=50000.50

echo "Name: $name"
echo "Age: $age"
echo "Name (alt): ${name}"
```

### 🔹 Naming Rules
✅ Valid:
```bash
first_name="John"
_last_name="Doe"
var1="test"
VAR2="constant"
```

❌ Invalid:
```bash
2var="invalid"      # Starts with number
my var="invalid"    # Contains space
var-name="invalid"  # Contains hyphen
```

**Rules:**
- Start with letter or underscore
- Can contain letters, numbers, underscores
- Case-sensitive
- No spaces around `=`

### 💡 Why Use `$` for Variables?
The `$` tells bash to reference the variable instead of a literal name.

Example:
```bash
name="Izhan"
echo name     # prints 'name'
echo $name    # prints 'Izhan'
```

---

## 🧮 Variable Types

```bash
#!/bin/bash

string1="Hello"
num1=10
fruits=("apple" "banana" "cherry")

echo "User: $USER"
echo "Home: $HOME"
```

---

## 💬 Quoting Mechanisms

```bash
#!/bin/bash

name="Alice"
age=30

echo "Hello, $name! You are $age years old."  # expands
echo 'Hello, $name! You are $age years old.'  # literal
```

---

## ⚙️ Command Substitution

```bash
#!/bin/bash
current_time=$(date +%H:%M:%S)
current_date=`date`

echo "Date: $current_date"
echo "Time: $current_time"
```

---

## 🧾 Special Variables

| Variable | Description |
|-----------|--------------|
| `$0` | Script name |
| `$1`, `$2`, ... | Positional arguments |
| `$@` | All arguments |
| `$#` | Argument count |
| `$$` | Process ID |
| `$?` | Exit status of last command |

---

## 🍎 Arrays

```bash
#!/bin/bash
fruits=("apple" "banana" "cherry" "date")
echo "First: ${fruits[0]}"
echo "All: ${fruits[@]}"
echo "Count: ${#fruits[@]}"

for fruit in "${fruits[@]}"; do
    echo "Fruit: $fruit"
done
```

### Associative Arrays (Bash 4+)
```bash
declare -A user
user["name"]="John"
user["age"]="25"

echo "User: ${user["name"]}, Age: ${user["age"]}"
```

---

## 🔢 Input / Output

```bash
#!/bin/bash

read -p "Enter your name: " username
echo "Hello, $username!"

read -s -p "Enter password: " password
echo

printf "Name: %s\n" "John"
printf "Age: %d\n" 25
```

---

## ➗ Arithmetic Operations

### Using `$(( ))`
```bash
a=10
b=3
echo "Sum: $((a + b))"
echo "Diff: $((a - b))"
echo "Mul: $((a * b))"
echo "Div: $((a / b))"
echo "Mod: $((a % b))"
echo "Pow: $((a ** b))"
```

### Using `expr`
```bash
expr 30 + 10
expr 30 \* 10
```

### Using `let`
```bash
let "sum = a + b"
let "product = a * b"
```

### Floating Point with `bc`
```bash
echo "scale=2; 10/3" | bc
# Output: 3.33
```

---

## ⚖️ Conditional Statements

```bash
number=10

if [ $number -eq 10 ]; then
    echo "Number is 10"
elif [ $number -gt 10 ]; then
    echo "Greater than 10"
else
    echo "Less than 10"
fi
```

### Numeric Comparison Operators
| Operator | Description |
|-----------|--------------|
| `-eq` | equal |
| `-ne` | not equal |
| `-gt` | greater than |
| `-lt` | less than |
| `-ge` | greater or equal |
| `-le` | less or equal |

### String Comparison Operators
| Operator | Description |
|-----------|--------------|
| `=` / `==` | equal |
| `!=` | not equal |
| `-z` | empty string |
| `-n` | not empty |

---

## 🧩 Case Statement

```bash
fruit="apple"
case $fruit in
  "apple") echo "It's an apple";;
  "banana") echo "It's a banana";;
  *) echo "Unknown fruit";;
esac
```

---

## 🔁 Loops

### For Loop
```bash
for i in {1..5}; do
  echo "Number: $i"
done

for ((i=1; i<=5; i++)); do
  echo "Counter: $i"
done

fruits=("apple" "banana" "cherry")
for fruit in "${fruits[@]}"; do
  echo "Fruit: $fruit"
done
```

### While Loop
```bash
counter=1
while [ $counter -le 5 ]; do
  echo "Counter: $counter"
  ((counter++))
done
```

### Until Loop
```bash
counter=1
until [ $counter -gt 5 ]; do
  echo "Counter: $counter"
  ((counter++))
done
```

### Break & Continue
```bash
for i in {1..10}; do
  if [ $i -eq 5 ]; then break; fi
  echo $i
done

for i in {1..5}; do
  if [ $i -eq 3 ]; then continue; fi
  echo $i
done
```



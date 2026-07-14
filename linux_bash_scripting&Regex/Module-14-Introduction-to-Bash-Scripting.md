# MODULE 14: Introduction to Bash Scripting

## Table of Contents
- [14.1 What is Bash Scripting?](#141-what-is-bash-scripting)
- [14.2 Your First Script](#142-your-first-script)
- [14.3 Variables](#143-variables)
- [14.4 User Input](#144-user-input)
- [14.5 Arithmetic Operations](#145-arithmetic-operations)
- [14.6 Conditional Statements (if/else)](#146-conditional-statements-ifelse)
- [14.7 Comparison Operators](#147-comparison-operators)
- [14.8 Case Statements](#148-case-statements)
- [14.9 Loops](#149-loops)
- [14.10 Functions](#1410-functions)
- [14.11 Working with Files in Scripts](#1411-working-with-files-in-scripts)
- [14.12 Command-Line Arguments](#1412-command-line-arguments)
- [14.13 Exit Codes and Error Handling](#1413-exit-codes-and-error-handling)
- [14.14 Practical Script Examples](#1414-practical-script-examples)
- [Summary](#summary)

---

## 14.1 What is Bash Scripting?

### Why Learn Bash Scripting?

A **Bash script** is a plain text file containing a series of commands that the Bash shell can execute automatically. Instead of typing commands one by one, you write them all in a file and run them together.

Think of it like a **recipe**:
- Instead of remembering every step, you write them down once
- Anyone can follow the recipe to get the same result
- You can reuse the recipe whenever you want

### What Can You Do with Bash Scripts?

```bash
# Automate repetitive tasks
# - Daily backups
# - Log file cleanup
# - User account creation

# System administration
# - Server monitoring
# - Service management
# - Security audits

# DevOps workflows
# - Deployment automation
# - Environment setup
# - CI/CD pipeline helpers
```

### Bash vs Other Scripting Languages

```
Language    | Best For                    | Learning Curve
-----------|-----------------------------|---------------
Bash       | System tasks, automation    | Easy
Python     | Complex logic, data work    | Medium
Perl       | Text processing             | Hard
PowerShell | Windows administration      | Medium
```

Bash scripting is ideal when you need to:
- Chain Linux commands together
- Automate system administration tasks
- Write quick, simple automation scripts

---

## 14.2 Your First Script

### Creating a Script File

```bash
# Step 1: Create a new file
touch hello.sh

# Step 2: Open with your favorite editor
nano hello.sh
# or
vim hello.sh
```

### The Shebang Line

Every script should start with a **shebang** (`#!`) line that tells the system which interpreter to use:

```bash
#!/bin/bash
# This tells the system: "Use /bin/bash to run this script"
```

### Your First Script: Hello World

```bash
#!/bin/bash
# My first Bash script
# Author: Your Name
# Date: 2025-01-15

echo "Hello, World!"
echo "Welcome to Bash Scripting!"
echo "Today is: $(date)"
echo "You are logged in as: $(whoami)"
echo "Current directory: $(pwd)"
```

### Making the Script Executable

```bash
# Method 1: Add execute permission
chmod +x hello.sh

# Run it
./hello.sh

# Method 2: Run with bash directly (no chmod needed)
bash hello.sh
```

### Output

```
Hello, World!
Welcome to Bash Scripting!
Today is: Wed Jan 15 10:30:00 UTC 2025
You are logged in as: john
Current directory: /home/john/scripts
```

### Script File Conventions

```bash
# 1. Use .sh extension (not required, but recommended)
myscript.sh
backup.sh
setup.sh

# 2. Use descriptive names
# Good:
backup-database.sh
create-user.sh
check-disk-space.sh

# Bad:
script1.sh
test.sh
x.sh

# 3. Always start with shebang
#!/bin/bash

# 4. Add comments to explain what the script does
#!/bin/bash
# Description: This script backs up the database
# Usage: ./backup-database.sh [database_name]
# Author: John Doe
```

---

## 14.3 Variables

### What are Variables?

Variables are **containers that store data**. Think of them like labeled boxes where you put information.

### Defining Variables

```bash
#!/bin/bash

# Simple variable assignment
# IMPORTANT: No spaces around the = sign!
name="John"
age=25
city="Istanbul"

# CORRECT:
name="John"

# WRONG (will cause error):
# name = "John"    # Spaces around =
# name ="John"     # Space before =
# name= "John"     # Space after =
```

### Using Variables

```bash
#!/bin/bash

# Use $ to access variable value
name="John"
age=25

echo "My name is $name"
echo "I am $age years old"

# Using curly braces (recommended for clarity)
echo "My name is ${name}"
echo "${name} is ${age} years old"

# When curly braces are NECESSARY
file="report"
echo "${file}_2025.txt"    # Output: report_2025.txt
echo "$file_2025.txt"      # WRONG: Tries to find variable $file_2025
```

### Variable Types

```bash
#!/bin/bash

# String variables
greeting="Hello World"
filename="backup.tar.gz"
path="/home/john/documents"

# Numeric variables
count=10
port=8080
max_retries=3

# Command output in a variable (Command Substitution)
current_date=$(date)
current_user=$(whoami)
file_count=$(ls | wc -l)
disk_usage=$(df -h / | tail -1 | awk '{print $5}')

echo "Date: $current_date"
echo "User: $current_user"
echo "Files in current directory: $file_count"
echo "Disk usage: $disk_usage"
```

### Environment Variables vs Local Variables

```bash
#!/bin/bash

# Local variable (only available in this script)
my_var="Hello"

# Environment variables (available system-wide)
echo "Home directory: $HOME"
echo "Current user: $USER"
echo "Shell: $SHELL"
echo "Path: $PATH"
echo "Hostname: $HOSTNAME"

# Make a local variable available to child processes
export MY_VAR="I am exported"
```

### Read-Only Variables

```bash
#!/bin/bash

# Create a read-only (constant) variable
readonly PI=3.14159
readonly APP_NAME="MyApp"

echo "Pi is $PI"
echo "App name is $APP_NAME"

# This will cause an error:
# PI=3.14    # Error: PI: readonly variable
```

### Quoting Variables

```bash
#!/bin/bash

name="John Doe"

# Double quotes: Variables are expanded
echo "Hello, $name"           # Output: Hello, John Doe

# Single quotes: Everything is literal
echo 'Hello, $name'           # Output: Hello, $name

# Best practice: Always quote your variables
filename="my file.txt"
# CORRECT:
cat "$filename"               # Handles spaces correctly

# WRONG:
# cat $filename               # Breaks on spaces (tries: cat my file.txt)
```

### Practical Example: System Info Script

```bash
#!/bin/bash
# system-info.sh - Display system information

hostname=$(hostname)
os_name=$(uname -s)
kernel=$(uname -r)
uptime_info=$(uptime -p)
cpu_cores=$(nproc)
total_mem=$(free -h | awk '/Mem:/ {print $2}')
disk_free=$(df -h / | awk 'NR==2 {print $4}')
ip_address=$(hostname -I | awk '{print $1}')

echo "========== System Information =========="
echo "Hostname     : $hostname"
echo "OS           : $os_name"
echo "Kernel       : $kernel"
echo "Uptime       : $uptime_info"
echo "CPU Cores    : $cpu_cores"
echo "Total Memory : $total_mem"
echo "Free Disk    : $disk_free"
echo "IP Address   : $ip_address"
echo "========================================"
```

---

## 14.4 User Input

### Reading Input with `read`

```bash
#!/bin/bash

# Simple input
echo "What is your name?"
read name
echo "Hello, $name!"

# Input with prompt (-p)
read -p "Enter your age: " age
echo "You are $age years old"

# Silent input for passwords (-s)
read -sp "Enter your password: " password
echo    # New line after hidden input
echo "Password received (length: ${#password})"

# Input with timeout (-t seconds)
read -t 10 -p "Enter your name (10 seconds): " name
if [ -z "$name" ]; then
    echo "Timed out! Using default."
    name="Guest"
fi
echo "Hello, $name!"

# Read multiple values
echo "Enter first and last name:"
read first_name last_name
echo "First: $first_name"
echo "Last: $last_name"
```

### Practical Example: Interactive Greeting Script

```bash
#!/bin/bash
# greeting.sh - Interactive greeting script

read -p "What is your name? " name
read -p "What is your favorite color? " color

echo ""
echo "Hello, $name!"
echo "Nice to know you like $color."
echo "Have a great day!"
```

### Practical Example: Simple Calculator (Input)

```bash
#!/bin/bash
# calculator.sh - Simple interactive calculator

read -p "Enter first number: " num1
read -p "Enter operator (+, -, *, /): " operator
read -p "Enter second number: " num2

case $operator in
    +) result=$(echo "$num1 + $num2" | bc) ;;
    -) result=$(echo "$num1 - $num2" | bc) ;;
    \*) result=$(echo "$num1 * $num2" | bc) ;;
    /)
        if [ "$num2" = "0" ]; then
            echo "Error: Division by zero!"
            exit 1
        fi
        result=$(echo "scale=2; $num1 / $num2" | bc)
        ;;
    *) echo "Invalid operator!"; exit 1 ;;
esac

echo "Result: $num1 $operator $num2 = $result"
```

---

## 14.5 Arithmetic Operations

### Methods for Arithmetic

```bash
#!/bin/bash

a=10
b=3

# Method 1: $(( )) - Double parentheses (RECOMMENDED)
sum=$((a + b))
diff=$((a - b))
mult=$((a * b))
div=$((a / b))        # Integer division only!
mod=$((a % b))        # Modulo (remainder)

echo "Sum: $sum"       # 13
echo "Diff: $diff"     # 7
echo "Mult: $mult"     # 30
echo "Div: $div"       # 3 (integer only!)
echo "Mod: $mod"       # 1

# Method 2: let command
let result=a+b
let "result = a + b"   # Spaces OK inside quotes
echo "Result: $result"  # 13

# Method 3: expr command (older method)
result=$(expr $a + $b)
echo "Result: $result"  # 13
# Note: * needs escaping with expr
result=$(expr $a \* $b)

# Method 4: bc - For decimal/floating-point math
result=$(echo "scale=2; 10 / 3" | bc)
echo "Result: $result"  # 3.33

result=$(echo "scale=4; 22 / 7" | bc)
echo "Pi approx: $result"  # 3.1428
```

### Increment and Decrement

```bash
#!/bin/bash

count=0

# Increment
((count++))    # count = 1
((count++))    # count = 2
echo "$count"  # 2

# Decrement
((count--))    # count = 1
echo "$count"  # 1

# Add/Subtract specific value
((count += 5))   # count = 6
((count -= 2))   # count = 4
((count *= 3))   # count = 12
echo "$count"    # 12
```

### Practical Example: Tip Calculator

```bash
#!/bin/bash
# tip-calculator.sh

read -p "Enter the bill amount: " bill
read -p "Enter tip percentage (e.g., 15): " tip_percent

tip_amount=$(echo "scale=2; $bill * $tip_percent / 100" | bc)
total=$(echo "scale=2; $bill + $tip_amount" | bc)

echo ""
echo "Bill     : $bill TL"
echo "Tip (%$tip_percent): $tip_amount TL"
echo "Total    : $total TL"
```

---

## 14.6 Conditional Statements (if/else)

### Basic if Statement

```bash
#!/bin/bash

age=18

# Basic if
if [ $age -ge 18 ]; then
    echo "You are an adult."
fi

# if-else
if [ $age -ge 18 ]; then
    echo "You can vote."
else
    echo "You cannot vote yet."
fi

# if-elif-else
if [ $age -lt 13 ]; then
    echo "You are a child."
elif [ $age -lt 18 ]; then
    echo "You are a teenager."
elif [ $age -lt 65 ]; then
    echo "You are an adult."
else
    echo "You are a senior."
fi
```

### Important Syntax Rules

```bash
# CORRECT:
if [ $age -ge 18 ]; then    # Spaces inside [ ] are REQUIRED!
    echo "Adult"
fi

# WRONG:
# if [$age -ge 18]; then    # Missing spaces inside brackets!
# if [ $age -ge 18 ]then    # Missing semicolon or newline before then!
```

### String Checks

```bash
#!/bin/bash

name="John"

# String equality
if [ "$name" = "John" ]; then
    echo "Hello, John!"
fi

# String inequality
if [ "$name" != "Admin" ]; then
    echo "You are not admin."
fi

# Empty string check
if [ -z "$name" ]; then
    echo "Name is empty"
else
    echo "Name is: $name"
fi

# Non-empty string check
if [ -n "$name" ]; then
    echo "Name is not empty"
fi
```

### File Checks

```bash
#!/bin/bash

file="/etc/passwd"
dir="/home/john"

# Check if file exists
if [ -f "$file" ]; then
    echo "$file exists and is a regular file"
fi

# Check if directory exists
if [ -d "$dir" ]; then
    echo "$dir exists and is a directory"
fi

# Check if file is readable
if [ -r "$file" ]; then
    echo "$file is readable"
fi

# Check if file is writable
if [ -w "$file" ]; then
    echo "$file is writable"
fi

# Check if file is executable
if [ -x "/usr/bin/bash" ]; then
    echo "bash is executable"
fi

# Check if file is NOT empty
if [ -s "$file" ]; then
    echo "$file is not empty"
fi

# Check if file or directory exists (any type)
if [ -e "$file" ]; then
    echo "$file exists"
fi
```

### Combining Conditions (AND / OR)

```bash
#!/bin/bash

age=25
name="John"

# AND: Both conditions must be true
if [ $age -ge 18 ] && [ "$name" = "John" ]; then
    echo "You are John and you are an adult."
fi

# OR: At least one condition must be true
if [ "$name" = "John" ] || [ "$name" = "Jane" ]; then
    echo "Hello, John or Jane!"
fi

# NOT: Reverse the condition
if [ ! -f "/tmp/lockfile" ]; then
    echo "Lock file does not exist."
fi

# Using double brackets [[ ]] (Bash-specific, more features)
if [[ $age -ge 18 && "$name" == "John" ]]; then
    echo "You are John and you are an adult."
fi

# Pattern matching with [[ ]]
filename="report.pdf"
if [[ "$filename" == *.pdf ]]; then
    echo "This is a PDF file"
fi
```

### Practical Example: File Checker Script

```bash
#!/bin/bash
# file-checker.sh - Check file properties

read -p "Enter file path: " filepath

if [ ! -e "$filepath" ]; then
    echo "Error: '$filepath' does not exist!"
    exit 1
fi

echo "=== File Information: $filepath ==="

if [ -f "$filepath" ]; then
    echo "Type: Regular file"
elif [ -d "$filepath" ]; then
    echo "Type: Directory"
elif [ -L "$filepath" ]; then
    echo "Type: Symbolic link"
else
    echo "Type: Other"
fi

[ -r "$filepath" ] && echo "Readable: Yes" || echo "Readable: No"
[ -w "$filepath" ] && echo "Writable: Yes" || echo "Writable: No"
[ -x "$filepath" ] && echo "Executable: Yes" || echo "Executable: No"

if [ -f "$filepath" ]; then
    size=$(stat --printf="%s" "$filepath" 2>/dev/null)
    echo "Size: $size bytes"
fi
```

---

## 14.7 Comparison Operators

### Numeric Comparison Operators

```bash
# Used inside [ ] or [[ ]]

# -eq   Equal to
# -ne   Not equal to
# -gt   Greater than
# -ge   Greater than or equal to
# -lt   Less than
# -le   Less than or equal to

#!/bin/bash

a=10
b=20

[ $a -eq $b ] && echo "Equal" || echo "Not equal"       # Not equal
[ $a -ne $b ] && echo "Not equal" || echo "Equal"       # Not equal
[ $a -gt $b ] && echo "a > b" || echo "a <= b"          # a <= b
[ $a -ge $b ] && echo "a >= b" || echo "a < b"          # a < b
[ $a -lt $b ] && echo "a < b" || echo "a >= b"          # a < b
[ $a -le $b ] && echo "a <= b" || echo "a > b"          # a <= b
```

### String Comparison Operators

```bash
# Used inside [ ] or [[ ]]

# =     Equal (use == inside [[ ]])
# !=    Not equal
# -z    String is empty
# -n    String is not empty
# <     Less than (alphabetically, inside [[ ]] only)
# >     Greater than (alphabetically, inside [[ ]] only)

#!/bin/bash

str1="hello"
str2="world"

[ "$str1" = "$str2" ] && echo "Same" || echo "Different"   # Different
[ "$str1" != "$str2" ] && echo "Different" || echo "Same"   # Different
[ -z "" ] && echo "Empty" || echo "Not empty"               # Empty
[ -n "$str1" ] && echo "Not empty" || echo "Empty"          # Not empty
```

### File Test Operators

```bash
# Common file test operators:

# -f file    True if file exists and is a regular file
# -d file    True if file exists and is a directory
# -e file    True if file exists (any type)
# -r file    True if file is readable
# -w file    True if file is writable
# -x file    True if file is executable
# -s file    True if file exists and is not empty
# -L file    True if file is a symbolic link
# -O file    True if you own the file
# -G file    True if file's group matches yours

# File comparison:
# file1 -nt file2    file1 is newer than file2
# file1 -ot file2    file1 is older than file2
```

### Quick Reference Table

```
Numeric:          String:           File:
-eq  (==)         =  (equals)      -f  (is file)
-ne  (!=)         != (not equal)   -d  (is directory)
-gt  (>)          -z (is empty)    -e  (exists)
-ge  (>=)         -n (not empty)   -r  (readable)
-lt  (<)                           -w  (writable)
-le  (<=)                          -x  (executable)
                                   -s  (not empty)
```

---

## 14.8 Case Statements

### Basic Syntax

```bash
#!/bin/bash
# Case statement - alternative to multiple if-elif

read -p "Enter a fruit: " fruit

case $fruit in
    apple)
        echo "Apples are red or green."
        ;;
    banana)
        echo "Bananas are yellow."
        ;;
    orange)
        echo "Oranges are orange."
        ;;
    *)
        echo "Unknown fruit: $fruit"
        ;;
esac
```

### Pattern Matching with Case

```bash
#!/bin/bash
# Case supports patterns

read -p "Enter a filename: " filename

case $filename in
    *.txt)
        echo "Text file"
        ;;
    *.jpg|*.png|*.gif)
        echo "Image file"
        ;;
    *.sh)
        echo "Shell script"
        ;;
    *.tar.gz|*.tgz)
        echo "Compressed archive"
        ;;
    *)
        echo "Unknown file type"
        ;;
esac
```

### Practical Example: Service Manager

```bash
#!/bin/bash
# service-menu.sh - Simple service manager

echo "=== Service Manager ==="
echo "1) Start service"
echo "2) Stop service"
echo "3) Restart service"
echo "4) Check status"
echo "5) Exit"
echo ""

read -p "Choose an option [1-5]: " choice

case $choice in
    1)
        read -p "Service name: " service
        echo "Starting $service..."
        # sudo systemctl start $service
        ;;
    2)
        read -p "Service name: " service
        echo "Stopping $service..."
        # sudo systemctl stop $service
        ;;
    3)
        read -p "Service name: " service
        echo "Restarting $service..."
        # sudo systemctl restart $service
        ;;
    4)
        read -p "Service name: " service
        echo "Checking status of $service..."
        # sudo systemctl status $service
        ;;
    5)
        echo "Goodbye!"
        exit 0
        ;;
    *)
        echo "Invalid option! Please choose 1-5."
        exit 1
        ;;
esac
```

### Practical Example: Day of the Week

```bash
#!/bin/bash
# day-info.sh

day=$(date +%A)

case $day in
    Monday)
        echo "Start of the work week!"
        ;;
    Tuesday|Wednesday|Thursday)
        echo "Midweek - keep going!"
        ;;
    Friday)
        echo "TGIF! Almost weekend!"
        ;;
    Saturday|Sunday)
        echo "Weekend! Time to relax."
        ;;
esac
```

---

## 14.9 Loops

### for Loop

```bash
#!/bin/bash

# Loop through a list
for color in red green blue yellow; do
    echo "Color: $color"
done
# Output:
# Color: red
# Color: green
# Color: blue
# Color: yellow

# Loop through a range
for i in {1..5}; do
    echo "Number: $i"
done
# Output: 1 2 3 4 5

# Loop with step
for i in {0..20..5}; do
    echo "Number: $i"
done
# Output: 0 5 10 15 20

# C-style for loop
for ((i=1; i<=5; i++)); do
    echo "Count: $i"
done

# Loop through files
for file in *.sh; do
    echo "Script: $file"
done

# Loop through command output
for user in $(cat /etc/passwd | cut -d: -f1 | head -5); do
    echo "User: $user"
done
```

### while Loop

```bash
#!/bin/bash

# Basic while loop
count=1
while [ $count -le 5 ]; do
    echo "Count: $count"
    ((count++))
done
# Output: 1 2 3 4 5

# Read a file line by line
while IFS= read -r line; do
    echo "Line: $line"
done < /etc/hostname

# Infinite loop with break
while true; do
    read -p "Enter 'quit' to exit: " input
    if [ "$input" = "quit" ]; then
        echo "Goodbye!"
        break
    fi
    echo "You said: $input"
done
```

### until Loop

```bash
#!/bin/bash

# until: runs UNTIL condition becomes true (opposite of while)
count=1
until [ $count -gt 5 ]; do
    echo "Count: $count"
    ((count++))
done
# Output: 1 2 3 4 5
```

### Loop Control: break and continue

```bash
#!/bin/bash

# break: Exit the loop entirely
for i in {1..10}; do
    if [ $i -eq 6 ]; then
        echo "Breaking at $i"
        break
    fi
    echo "Number: $i"
done
# Output: 1 2 3 4 5 Breaking at 6

# continue: Skip current iteration, go to next
for i in {1..10}; do
    if [ $((i % 2)) -eq 0 ]; then
        continue    # Skip even numbers
    fi
    echo "Odd number: $i"
done
# Output: 1 3 5 7 9
```

### Practical Example: Countdown Timer

```bash
#!/bin/bash
# countdown.sh

read -p "Enter countdown seconds: " seconds

while [ $seconds -gt 0 ]; do
    echo -ne "\rTime remaining: ${seconds}s  "
    sleep 1
    ((seconds--))
done

echo -e "\rTime's up!              "
```

### Practical Example: Batch File Renamer

```bash
#!/bin/bash
# rename-files.sh - Add prefix to all .txt files

read -p "Enter prefix to add: " prefix

count=0
for file in *.txt; do
    if [ -f "$file" ]; then
        mv "$file" "${prefix}_${file}"
        echo "Renamed: $file -> ${prefix}_${file}"
        ((count++))
    fi
done

echo "Total files renamed: $count"
```

### Practical Example: Multiplication Table

```bash
#!/bin/bash
# multiplication-table.sh

read -p "Enter a number: " num

echo "=== Multiplication Table for $num ==="
for i in {1..10}; do
    result=$((num * i))
    echo "$num x $i = $result"
done
```

---

## 14.10 Functions

### Defining and Calling Functions

```bash
#!/bin/bash

# Method 1: Standard syntax
greet() {
    echo "Hello, World!"
}

# Method 2: With 'function' keyword
function say_goodbye() {
    echo "Goodbye, World!"
}

# Calling functions
greet
say_goodbye
```

### Functions with Parameters

```bash
#!/bin/bash

# Parameters are accessed with $1, $2, $3, etc.
greet_user() {
    echo "Hello, $1!"
    echo "You are $2 years old."
}

# Call with arguments
greet_user "John" 25
# Output:
# Hello, John!
# You are 25 years old.

# Function with multiple parameters
create_file() {
    local filename=$1
    local content=$2

    echo "$content" > "$filename"
    echo "Created file: $filename"
}

create_file "note.txt" "This is my note"
```

### Return Values

```bash
#!/bin/bash

# Functions return exit codes (0-255), not values
# Use echo to "return" values

# Return exit code
is_even() {
    if [ $(($1 % 2)) -eq 0 ]; then
        return 0    # Success (true)
    else
        return 1    # Failure (false)
    fi
}

if is_even 4; then
    echo "4 is even"
fi

# Return a value using echo
get_square() {
    local num=$1
    echo $((num * num))
}

result=$(get_square 5)
echo "Square of 5 is: $result"    # 25
```

### Local Variables

```bash
#!/bin/bash

# Variables inside functions are GLOBAL by default!
# Use 'local' to make them function-scoped

my_function() {
    local local_var="I am local"
    global_var="I am global"
    echo "Inside function: $local_var"
}

my_function
echo "Outside function: $global_var"     # Works
echo "Outside function: $local_var"      # Empty (local only)
```

### Practical Example: Logging Function

```bash
#!/bin/bash
# Functions to standardize logging

log_info() {
    echo "[INFO]  $(date '+%Y-%m-%d %H:%M:%S') - $1"
}

log_warn() {
    echo "[WARN]  $(date '+%Y-%m-%d %H:%M:%S') - $1"
}

log_error() {
    echo "[ERROR] $(date '+%Y-%m-%d %H:%M:%S') - $1" >&2
}

# Usage
log_info "Script started"
log_info "Processing files..."
log_warn "Disk space is running low"
log_error "Failed to connect to database"
log_info "Script finished"

# Output:
# [INFO]  2025-01-15 10:30:00 - Script started
# [INFO]  2025-01-15 10:30:00 - Processing files...
# [WARN]  2025-01-15 10:30:00 - Disk space is running low
# [ERROR] 2025-01-15 10:30:00 - Failed to connect to database
# [INFO]  2025-01-15 10:30:01 - Script finished
```

---

## 14.11 Working with Files in Scripts

### Checking Files

```bash
#!/bin/bash
# Common file operations in scripts

# Check before operating on a file
backup_file() {
    local source=$1

    if [ ! -f "$source" ]; then
        echo "Error: File '$source' not found!"
        return 1
    fi

    cp "$source" "${source}.bak"
    echo "Backup created: ${source}.bak"
}

backup_file "/etc/hosts"
```

### Reading Files

```bash
#!/bin/bash

# Read file line by line
while IFS= read -r line; do
    echo "Processing: $line"
done < input.txt

# Read file into an array
mapfile -t lines < input.txt
echo "Total lines: ${#lines[@]}"
echo "First line: ${lines[0]}"
echo "Last line: ${lines[-1]}"

# Read specific fields (CSV-like)
while IFS=, read -r name age city; do
    echo "Name: $name, Age: $age, City: $city"
done < data.csv
```

### Writing to Files

```bash
#!/bin/bash

# Write to file (overwrite)
echo "Hello World" > output.txt

# Append to file
echo "Another line" >> output.txt

# Write multiple lines (heredoc)
cat > config.txt << EOF
# Configuration File
hostname=server01
port=8080
debug=false
EOF

# Write multiple lines (append)
cat >> config.txt << EOF
# Additional settings
log_level=info
EOF
```

### Practical Example: Log Analyzer

```bash
#!/bin/bash
# log-analyzer.sh - Simple log file analyzer

logfile=$1

if [ -z "$logfile" ]; then
    echo "Usage: $0 <logfile>"
    exit 1
fi

if [ ! -f "$logfile" ]; then
    echo "Error: File '$logfile' not found!"
    exit 1
fi

total_lines=$(wc -l < "$logfile")
error_count=$(grep -ci "error" "$logfile")
warn_count=$(grep -ci "warn" "$logfile")

echo "=== Log Analysis: $logfile ==="
echo "Total lines : $total_lines"
echo "Errors      : $error_count"
echo "Warnings    : $warn_count"
echo "=============================="
```

---

## 14.12 Command-Line Arguments

### Accessing Arguments

```bash
#!/bin/bash
# arguments.sh - Demonstrate command-line arguments

echo "Script name : $0"
echo "First arg   : $1"
echo "Second arg  : $2"
echo "Third arg   : $3"
echo "All args    : $@"
echo "Arg count   : $#"
echo "All as one  : $*"

# Run: ./arguments.sh hello world 42
# Output:
# Script name : ./arguments.sh
# First arg   : hello
# Second arg  : world
# Third arg   : 42
# All args    : hello world 42
# Arg count   : 3
# All as one  : hello world 42
```

### Special Variables Reference

```bash
# $0    Script name
# $1-$9 Positional parameters (arguments)
# $#    Number of arguments
# $@    All arguments (as separate words)
# $*    All arguments (as a single string)
# $?    Exit status of last command
# $$    Process ID of current script
# $!    Process ID of last background command
```

### Validating Arguments

```bash
#!/bin/bash
# validate-args.sh

# Check if enough arguments provided
if [ $# -lt 2 ]; then
    echo "Usage: $0 <source> <destination>"
    echo "Example: $0 /home/john/file.txt /backup/"
    exit 1
fi

source=$1
destination=$2

echo "Copying '$source' to '$destination'..."
cp "$source" "$destination"
echo "Done!"
```

### Iterating Over Arguments

```bash
#!/bin/bash
# process-files.sh - Process multiple files

if [ $# -eq 0 ]; then
    echo "Usage: $0 file1 [file2] [file3] ..."
    exit 1
fi

for file in "$@"; do
    if [ -f "$file" ]; then
        lines=$(wc -l < "$file")
        size=$(stat --printf="%s" "$file")
        echo "$file: $lines lines, $size bytes"
    else
        echo "$file: NOT FOUND"
    fi
done
```

---

## 14.13 Exit Codes and Error Handling

### Understanding Exit Codes

```bash
#!/bin/bash

# Every command returns an exit code:
# 0     = Success
# 1-255 = Error (different codes for different errors)

# Check exit code with $?
ls /etc/passwd
echo "Exit code: $?"    # 0 (success)

ls /nonexistent
echo "Exit code: $?"    # 2 (no such file)
```

### Setting Exit Codes

```bash
#!/bin/bash
# deploy.sh

if [ $# -eq 0 ]; then
    echo "Error: No environment specified!"
    exit 1    # Exit with error code 1
fi

environment=$1

case $environment in
    dev|staging|prod)
        echo "Deploying to $environment..."
        exit 0    # Success
        ;;
    *)
        echo "Error: Unknown environment '$environment'"
        echo "Use: dev, staging, or prod"
        exit 2    # Different error code
        ;;
esac
```

### Error Handling with set

```bash
#!/bin/bash

# Exit immediately if a command fails
set -e

# Treat unset variables as errors
set -u

# Fail on pipe errors
set -o pipefail

# Combine all three (recommended for scripts)
set -euo pipefail

# Example with error handling
echo "Creating directory..."
mkdir -p /tmp/myproject

echo "Entering directory..."
cd /tmp/myproject

echo "Downloading file..."
# If curl fails, script stops here (because of set -e)
# curl -O https://example.com/file.tar.gz

echo "Done!"
```

### Trap: Cleanup on Exit

```bash
#!/bin/bash
# Script with cleanup

# Create a temp file
tmpfile=$(mktemp)

# Set up cleanup - runs when script exits (normally or on error)
cleanup() {
    echo "Cleaning up temporary files..."
    rm -f "$tmpfile"
}
trap cleanup EXIT

# Use the temp file
echo "Working with temp file: $tmpfile"
echo "some data" > "$tmpfile"

# Even if something fails, cleanup runs automatically
```

### Practical Example: Safe Script Template

```bash
#!/bin/bash
# template.sh - A safe script template

set -euo pipefail

# Constants
readonly SCRIPT_NAME=$(basename "$0")
readonly LOG_FILE="/tmp/${SCRIPT_NAME}.log"

# Logging
log() {
    echo "[$(date '+%Y-%m-%d %H:%M:%S')] $1" | tee -a "$LOG_FILE"
}

# Cleanup
cleanup() {
    log "Script finished. Cleaning up..."
}
trap cleanup EXIT

# Argument validation
if [ $# -lt 1 ]; then
    echo "Usage: $SCRIPT_NAME <argument>"
    exit 1
fi

# Main logic
log "Script started with argument: $1"
log "Performing operations..."

# Your code here

log "All operations completed successfully."
```

---

## 14.14 Practical Script Examples

### Example 1: Disk Space Alert

```bash
#!/bin/bash
# disk-alert.sh - Alert when disk usage exceeds threshold

threshold=80

df -H | awk 'NR>1 {print $5 " " $6}' | while read -r usage mount; do
    # Remove % sign
    usage_num=${usage%\%}

    if [ "$usage_num" -gt "$threshold" ]; then
        echo "WARNING: $mount is ${usage} full!"
    fi
done
```

### Example 2: Backup Script

```bash
#!/bin/bash
# backup.sh - Simple directory backup

set -euo pipefail

# Configuration
SOURCE_DIR=${1:-"/home/$USER/documents"}
BACKUP_DIR="/tmp/backups"
DATE=$(date +%Y%m%d_%H%M%S)
BACKUP_FILE="backup_${DATE}.tar.gz"

# Create backup directory if it doesn't exist
mkdir -p "$BACKUP_DIR"

# Check source directory
if [ ! -d "$SOURCE_DIR" ]; then
    echo "Error: Source directory '$SOURCE_DIR' not found!"
    exit 1
fi

# Create backup
echo "Creating backup of $SOURCE_DIR..."
tar -czf "${BACKUP_DIR}/${BACKUP_FILE}" -C "$(dirname "$SOURCE_DIR")" "$(basename "$SOURCE_DIR")"

# Verify
if [ -f "${BACKUP_DIR}/${BACKUP_FILE}" ]; then
    size=$(du -h "${BACKUP_DIR}/${BACKUP_FILE}" | cut -f1)
    echo "Backup successful!"
    echo "File: ${BACKUP_DIR}/${BACKUP_FILE}"
    echo "Size: $size"
else
    echo "Backup failed!"
    exit 1
fi
```

### Example 3: User Account Creator

```bash
#!/bin/bash
# create-users.sh - Create multiple users from a file

# Input file format: username,fullname,group
# Example: john,John Smith,developers

set -euo pipefail

if [ $# -ne 1 ]; then
    echo "Usage: $0 <users_file>"
    echo "File format: username,fullname,group"
    exit 1
fi

USERS_FILE=$1

if [ ! -f "$USERS_FILE" ]; then
    echo "Error: File '$USERS_FILE' not found!"
    exit 1
fi

while IFS=, read -r username fullname group; do
    # Skip empty lines and comments
    [[ -z "$username" || "$username" =~ ^# ]] && continue

    # Check if user exists
    if id "$username" &>/dev/null; then
        echo "SKIP: User '$username' already exists"
        continue
    fi

    # Create group if it doesn't exist
    if ! getent group "$group" &>/dev/null; then
        echo "Creating group: $group"
        sudo groupadd "$group"
    fi

    # Create user
    echo "Creating user: $username ($fullname) in group: $group"
    sudo useradd -m -c "$fullname" -g "$group" "$username"

    echo "User '$username' created successfully"
done < "$USERS_FILE"

echo "All users processed!"
```

### Example 4: System Health Check

```bash
#!/bin/bash
# health-check.sh - Basic system health check

echo "====================================="
echo "    SYSTEM HEALTH CHECK REPORT"
echo "    $(date '+%Y-%m-%d %H:%M:%S')"
echo "====================================="
echo ""

# 1. System Uptime
echo "--- Uptime ---"
uptime -p
echo ""

# 2. CPU Load
echo "--- CPU Load ---"
load=$(uptime | awk -F'load average:' '{print $2}')
echo "Load Average: $load"
echo ""

# 3. Memory Usage
echo "--- Memory Usage ---"
free -h | awk 'NR==2{printf "Used: %s / %s (%.1f%%)\n", $3, $2, $3/$2*100}'
echo ""

# 4. Disk Usage
echo "--- Disk Usage ---"
df -h / | awk 'NR==2{printf "Used: %s / %s (%s)\n", $3, $2, $5}'
echo ""

# 5. Top 5 Processes (by CPU)
echo "--- Top 5 Processes (CPU) ---"
ps aux --sort=-%cpu | head -6 | awk '{printf "%-10s %5s%% %5s%% %s\n", $1, $3, $4, $11}'
echo ""

# 6. Logged-in Users
echo "--- Logged-in Users ---"
who | awk '{print $1, "from", $5, "since", $3, $4}'
echo ""

# 7. Network Status
echo "--- Network ---"
ip -4 addr show | grep inet | awk '{print $NF, $2}'
echo ""

echo "====================================="
echo "    Health check completed!"
echo "====================================="
```

### Example 5: Menu-Driven System Tool

```bash
#!/bin/bash
# system-tool.sh - Interactive system administration tool

show_menu() {
    echo ""
    echo "==============================="
    echo "   System Administration Tool"
    echo "==============================="
    echo "1) Show system info"
    echo "2) Show disk usage"
    echo "3) Show memory usage"
    echo "4) Show active users"
    echo "5) Show running services"
    echo "6) Search for a file"
    echo "0) Exit"
    echo "==============================="
}

while true; do
    show_menu
    read -p "Select option [0-6]: " choice

    case $choice in
        1)
            echo ""
            echo "Hostname: $(hostname)"
            echo "OS: $(uname -o)"
            echo "Kernel: $(uname -r)"
            echo "Uptime: $(uptime -p)"
            ;;
        2)
            echo ""
            df -h | head -10
            ;;
        3)
            echo ""
            free -h
            ;;
        4)
            echo ""
            who
            ;;
        5)
            echo ""
            systemctl list-units --type=service --state=running | head -20
            ;;
        6)
            read -p "Enter filename to search: " fname
            echo "Searching..."
            find /home -name "$fname" 2>/dev/null
            ;;
        0)
            echo "Goodbye!"
            exit 0
            ;;
        *)
            echo "Invalid option!"
            ;;
    esac
done
```

---

## Summary

### Script Structure

```bash
#!/bin/bash
# Description: What this script does
# Usage: ./script.sh [arguments]
# Author: Your Name

set -euo pipefail        # Safety settings

# Variables
MY_VAR="value"

# Functions
my_function() {
    local param=$1
    echo "$param"
}

# Main logic
if [ $# -lt 1 ]; then
    echo "Usage: $0 <argument>"
    exit 1
fi

my_function "$1"
```

### Key Concepts

1. **Shebang**: `#!/bin/bash` - tells the system which interpreter to use
2. **Variables**: `name="value"` - no spaces around `=`
3. **User Input**: `read -p "Prompt: " variable`
4. **Arithmetic**: `$((expression))` or `bc` for decimals
5. **Conditions**: `if [ condition ]; then ... fi`
6. **Case**: `case $var in pattern) ... ;; esac`
7. **For Loop**: `for item in list; do ... done`
8. **While Loop**: `while [ condition ]; do ... done`
9. **Functions**: `func_name() { ... }`
10. **Arguments**: `$1, $2, $#, $@`
11. **Exit Codes**: `0` = success, `1-255` = error

### Variable Quick Reference

```bash
$0             # Script name
$1, $2...      # Positional arguments
$#             # Number of arguments
$@             # All arguments (separate words)
$?             # Last command's exit code
$$             # Current script's PID
$USER          # Current username
$HOME          # Home directory
$PWD           # Current directory
$(command)     # Command substitution
```

### Comparison Operators Quick Reference

```
Numbers:            Strings:            Files:
-eq  (equal)        =   (equal)         -f (is file)
-ne  (not equal)    !=  (not equal)     -d (is directory)
-gt  (greater)      -z  (empty)         -e (exists)
-ge  (greater/eq)   -n  (not empty)     -r (readable)
-lt  (less)                             -w (writable)
-le  (less/eq)                          -x (executable)
```

### Best Practices

```bash
# 1. Always use shebang
#!/bin/bash

# 2. Use safety settings
set -euo pipefail

# 3. Quote your variables
echo "$variable"    # Not: echo $variable

# 4. Use local in functions
my_func() {
    local my_var="value"
}

# 5. Validate arguments
if [ $# -lt 1 ]; then
    echo "Usage: $0 <arg>"
    exit 1
fi

# 6. Use meaningful variable names
backup_directory="/backups"    # Not: bd="/backups"

# 7. Add comments for clarity
# Calculate disk usage percentage
usage=$((used * 100 / total))

# 8. Use functions for reusable code
log() { echo "[$(date)] $1"; }

# 9. Handle errors gracefully
if ! command -v curl &>/dev/null; then
    echo "Error: curl is not installed"
    exit 1
fi

# 10. Clean up temporary files
trap 'rm -f /tmp/myscript_*' EXIT
```

---

**Happy Scripting!**

Mastering User Input in Bash Scripts

Every interactive program needs to get input from users. In Bash, the `read` command is your primary tool for capturing user input and storing it in variables. This guide will show you everything you need to know about getting user input effectively.

## The `read` Command: Your Input Gateway

The `read` command is Bash's built-in tool for getting input from users. It pauses script execution and waits for the user to type something and press Enter.

### Basic Syntax
```bash
read variable_name
```

### Simple Example
```bash
read name
# User types: John
# Variable $name now contains: John
```

## How `read` Works

### Single Variable Input
When you provide one variable name, `read` captures the entire line:
```bash
read user_input
Hello World!  # User enters this
# $user_input = "Hello World!"
```

### Multiple Variables Input
When you provide multiple variable names, `read` splits the input by whitespace:
```bash
read name age city
John 30 London  # User enters this
# $name = "John"
# $age = "30"
# $city = "London"
```

### Word Splitting Rules
- **Default delimiter**: Whitespace (spaces, tabs)
- **IFS variable**: Controls how input is split
- **Last variable**: Gets all remaining words

```bash
read first second third
one two three four five
# $first = "one"
# $second = "two"
# $third = "three four five"  # All remaining words
```

## The REPLY Variable

If you don't specify a variable name, `read` uses the predefined `REPLY` variable:
```bash
read
100  # User enters this
# $REPLY = "100"
```

## Adding Prompts with `-p` Option

The `-p` option displays a prompt message before waiting for input:
```bash
read -p "Enter your name: " name
# Displays: Enter your name: 
# Then waits for input
```

### Practical Example: IP Blocker Script
```bash
#!/bin/bash
read -p "Enter the IP address or domain to block: " IP
sudo iptables -I INPUT -s $IP -j DROP
```

## Hiding Input with `-s` Option

The `-s` option suppresses echo, perfect for passwords:
```bash
read -s -p "Enter password: " password
# User types: secret123
# Nothing appears on screen
# $password = "secret123"
```

## Complete `read` Options Reference

| Option | Description |
|--------|-------------|
| `-p "prompt"` | Display a prompt |
| `-s` | Silent mode (no echo) |
| `-a array` | Read into an array |
| `-n num` | Read specific number of characters |
| `-t seconds` | Timeout after N seconds |
| `-r` | Raw input (backslashes not interpreted) |

## Real-World Examples

### 1. Interactive Configuration
```bash
#!/bin/bash
read -p "Database host [localhost]: " db_host
db_host=${db_host:-localhost}  # Default value

read -p "Database name: " db_name
read -p "Username: " db_user
read -s -p "Password: " db_pass
echo  # New line after hidden input
```

### 2. Menu Selection
```bash
#!/bin/bash
echo "1. Start service"
echo "2. Stop service"
echo "3. Restart service"
read -p "Choose an option: " choice

case $choice in
    1) echo "Starting..." ;;
    2) echo "Stopping..." ;;
    3) echo "Restarting..." ;;
    *) echo "Invalid option" ;;
esac
```

### 3. Validation Loop
```bash
#!/bin/bash
while true; do
    read -p "Enter yes or no: " answer
    if [[ "$answer" == "yes" || "$answer" == "no" ]]; then
        break
    fi
    echo "Please enter 'yes' or 'no'"
done
```

## Advanced Techniques

### Reading into an Array
```bash
read -a colors
red green blue  # User enters this
# colors[0] = "red"
# colors[1] = "green"
# colors[2] = "blue"
```

### Character Limit
```bash
read -n 3 -p "Enter 3 digits: " digits
# User can only type 3 characters
```

### Timeout
```bash
read -t 5 -p "You have 5 seconds to respond: " response
# Script continues after 5 seconds if no input
```

### Raw Mode
```bash
read -r -p "Enter path: " path
# Backslashes are treated as literal characters
```

## Common Pitfalls and Solutions

### Problem: Spaces in Input
```bash
# This won't work as expected:
read first last
John Doe
# $first = "John", $last = "Doe"

# Solution for full name:
read -p "Enter name: " full_name
John Doe
# $full_name = "John Doe"
```

### Problem: Empty Input
```bash
read -p "Enter value: " value
# User just presses Enter
# $value is empty

# Solution with validation:
while [[ -z "$value" ]]; do
    read -p "Enter value (cannot be empty): " value
done
```

### Problem: Special Characters
```bash
read -p "Enter filename: " filename
file with spaces.txt
# This can cause issues in commands

# Solution: Always quote variables
touch "$filename"
```

## Best Practices

1. **Always provide clear prompts** - Tell users what you expect
2. **Use defaults when possible** - Show them in brackets
3. **Validate input** - Don't trust user input
4. **Use `-s` for sensitive data** - Passwords, API keys
5. **Quote your variables** - Prevent word splitting issues
6. **Provide feedback** - Confirm what was entered

## Summary

- **`read`** - The primary command for user input
- **`-p`** - Adds a prompt message
- **`-s`** - Hides input (for passwords)
- **`REPLY`** - Default variable when none specified
- **Multiple variables** - Splits input by whitespace
- **Always quote** - `$variable` should be `"$variable"`

## What's Next?

Now that you know how to get user input, you're ready to learn about **special variables and positional parameters** - another powerful way to pass data to your scripts!
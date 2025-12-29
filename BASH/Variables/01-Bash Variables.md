Bash Variables: The Foundation of Shell Scripting

Variables are fundamental building blocks in any programming language, and Bash is no exception. In this comprehensive guide, you'll learn everything you need to know about creating, managing, and naming variables in Bash scripts.

## What Are Variables?

A variable is simply a **named memory location** that stores a value you can manipulate. Think of it as a labeled container where you can store data for later use in your scripts.

## Creating Variables in Bash

### Basic Syntax
```bash
variable_name=value
```

### Key Rules:
- **No spaces around the equals sign** - this is critical!
- The variable name comes first
- The value follows immediately after the equals sign

### Example:
```bash
os=linux
```

### Common Mistake:
```bash
os = linux  # ❌ WRONG - this will fail!
```

When you add spaces, Bash interprets `os` as a command and tries to execute it, resulting in a "command not found" error.

## Handling Values with Spaces

When your variable value contains spaces, you **must** enclose it in quotes:

```bash
distro="Kali Linux"  # ✅ Correct
distro=Kali Linux    # ❌ Wrong - will cause errors
```

## Variable Types in Bash

Bash is a **weakly typed language**, meaning:
- You don't need to declare data types
- The type is determined by the value assigned
- Bash automatically handles the conversion

### String Variables:
```bash
name="John Doe"
os="linux"
```

### Integer Variables:
```bash
age=30
count=100
```

### Important Limitation:
Bash **does not support floating-point numbers**. You can only work with integers.

## Variable Expansion and Combining Variables

You can create variables that reference other variables:

```bash
os=linux
distro=Ubuntu
mydistro="$os $distro"  # Results in "linux Ubuntu"
```

## Managing Variables

### Viewing All Variables
To see all shell variables and functions:
```bash
set
```

Since this produces a lot of output, use `grep` to search:
```bash
set | grep distro
```

### Removing Variables
To delete a variable:
```bash
unset variable_name
```

Example:
```bash
echo $distro      # Shows current value
unset distro
echo $distro      # Returns empty (variable is gone)
```

## Read-Only Variables (Constants)

Sometimes you need to ensure a variable's value never changes. Use the `declare` command with the `-r` flag:

```bash
declare -r LOG_DIR="/var/log"
```

### Characteristics of Read-Only Variables:
- **Cannot be modified** after declaration
- **Cannot be unset**
- **Useful for constants** that should remain fixed

Example:
```bash
declare -r SECONDS_PER_HOUR=3600
SECONDS_PER_HOUR=7200  # ❌ Error: is read only
unset SECONDS_PER_HOUR  # ❌ Error: cannot unset
```

## Variable Naming Conventions

### Best Practices:

**For User Variables:**
- Use **lowercase** letters
- Use **underscores** to separate words
- Make names **descriptive**
- Examples:
  ```bash
  user_name="john"
  file_count=42
  log_directory="/var/log"
  ```

**For Constants and Environment Variables:**
- Use **UPPERCASE** letters
- Use **underscores** to separate words
- Declare at the top of your script
- Examples:
  ```bash
  declare -r MAX_RETRIES=3
  PATH="/usr/bin:/bin"
  ```

### Invalid Variable Names:
```bash
2users=true        # ❌ Starts with a number
file.name=100      # ❌ Contains a dot
user@name=100      # ❌ Contains @ symbol
```

### Valid Variable Names:
```bash
user4=false        # ✅
server_name="apex" # ✅
A=67               # ✅
_underscore=100    # ✅ Can start with underscore
```

## Summary: Key Takeaways

- **Syntax**: `variable_name=value` (no spaces around `=`)
- **Strings with spaces**: Always use quotes: `variable="value with spaces"`
- **No type declarations**: Bash handles types automatically
- **No floating-point**: Only integers are supported
- **Use `set`**: To view all variables
- **Use `unset`**: To remove variables
- **Use `declare -r`**: To create read-only constants
- **Naming**: Lowercase for variables, UPPERCASE for constants

## What's Next?

Now that you understand variables, you're ready to explore **variable expansion and quoting** - two essential concepts that will help you handle complex string operations and avoid common pitfalls in your scripts.
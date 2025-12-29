Bash Variable Expansion and Quoting: A Complete Guide

Variable expansion and quoting are fundamental concepts in Bash scripting that determine how your variables are interpreted and how special characters are processed. Mastering these concepts is essential for writing robust, error-free scripts.

## Variable Expansion: Getting Values from Variables

Variable expansion is the process of replacing a variable name with its value. The dollar sign (`$`) triggers this expansion.

### Basic Variable Reference
```bash
age=30
os="Kali Linux"

echo $age      # Output: 30
echo $os       # Output: Kali Linux
```

### The Curly Brace Syntax
Bash offers two ways to reference variables:
- **Simple form**: `$variable`
- **Curly brace form**: `${variable}`

Both work for basic cases, but curly braces are more explicit and necessary in certain situations.

### When to Use Curly Braces

**1. Appending to Variables**
```bash
os=Windows
echo $os11     # Output: (empty) - looks for variable "os11"
echo ${os}11   # Output: Windows11 - correctly references "os"
```

**2. Avoiding Ambiguity**
```bash
filename="document"
echo "${filename}_backup.txt"  # Output: document_backup.txt
```

### Undefined Variables
Unlike many programming languages, referencing an undefined variable in Bash doesn't produce an error:
```bash
echo $undefined_var  # Output: (empty string)
echo ${undefined_var} # Output: (empty string)
```

## Quoting Mechanisms in Bash

Quoting controls how Bash interprets special characters. There are three quoting mechanisms:

### 1. Single Quotes (Literal Quotes)
Single quotes preserve the **literal value** of every character within them.

```bash
echo '$HOME and $PATH'  # Output: $HOME and $PATH
```

**Key characteristics:**
- No variable expansion occurs
- No command substitution
- No arithmetic expansion
- Everything is treated as literal text

**Limitation:** You cannot include a single quote within single quotes, even with backslashes.

### 2. Double Quotes (Interpretive Quotes)
Double quotes preserve most characters but allow certain expansions.

```bash
echo "$HOME and $PATH"  # Output: /home/user and /usr/bin:/bin
```

**What happens inside double quotes:**
- ✅ Variable expansion (`$variable`)
- ✅ Command substitution (`` `command` `` or `$(command)`)
- ✅ Arithmetic expansion (`$((expression))`)
- ❌ Most other special characters are preserved literally

**Special cases within double quotes:**
- You can include single quotes freely
- To include literal double quotes, escape them: `\"`
- Backslashes retain special meaning only before `$`, `` ` ``, `"`, `\`, and newline

### 3. Backslash (Escape Character)
The backslash removes the special meaning of the character that follows it.

```bash
echo \$HOME      # Output: $HOME
echo "Hello\n"   # Output: Hello\n (backslash before n is not special)
```

## Practical Examples and Common Pitfalls

### Problem: Special Characters
```bash
# This will fail:
echo 50% off

# Solutions:
echo '50% off'           # Single quotes
echo "50% off"           # Double quotes  
echo 50\% off            # Escape the %
```

### Problem: Windows Paths
```bash
# This won't work as expected:
echo C:\Windows\System32

# Solutions:
echo "C:\Windows\System32"          # Double quotes
echo C:\\Windows\\System32          # Double backslashes
```

### Problem: Command Backgrounding
```bash
# This runs 'me' in background and tries to execute 'you':
echo me and you

# Solutions:
echo 'me and you'        # Single quotes
echo "me and you"        # Double quotes  
echo me\ and\ you        # Escape spaces and &
```

## Quoting Comparison Table

| Feature | Single Quotes | Double Quotes | Backslash |
|---------|---------------|---------------|-----------|
| **Variable expansion** | ❌ No | ✅ Yes | ❌ No |
| **Command substitution** | ❌ No | ✅ Yes | ❌ No |
| **Arithmetic expansion** | ❌ No | ✅ Yes | ❌ No |
| **Special characters** | All literal | Most literal | Next char only |
| **Nesting quotes** | Not allowed | Single inside OK | N/A |

## Best Practices for Quoting

### 1. When to Use Single Quotes
- When you want literal text
- When your string contains variables that should NOT expand
- When you're unsure, start with single quotes

```bash
echo 'The variable $HOME contains: $HOME'
```

### 2. When to Use Double Quotes
- When you need variable expansion
- When you want to embed single quotes
- When building strings with dynamic content

```bash
echo "Current user: $USER, Home: $HOME"
```

### 3. When to Use Backslashes
- For escaping individual characters
- In command-line situations where quotes aren't available
- For line continuation (backslash at end of line)

```bash
echo \$PATH is \$PATH
```

## Advanced Quoting Scenarios

### Nested Quotes
```bash
# Single quotes inside double quotes:
echo "He said 'Hello World'"

# Double quotes inside double quotes (escape):
echo "She said \"Hello World\""
```

### Multiple Variables in One String
```bash
name="John"
age=30
echo "$name is $age years old"  # John is 30 years old
```

### Quoting with Commands
```bash
# Command substitution with quotes:
echo "Current directory: $(pwd)"
echo "Today is `date`"
```

## Summary: Key Takeaways

- **Single quotes** = "What you see is what you get" - literal values only
- **Double quotes** = "Interpret some things" - allows variable/command expansion
- **Backslash** = "Escape the next character" - removes special meaning
- **Curly braces** = Use when appending or for clarity: `${variable}`
- **Undefined variables** = Expand to empty string, no errors

## What's Next?

Now that you understand how to properly handle variables and quoting, you're ready to explore **environment variables** - a special type of variable that persists across sessions and can be inherited by child processes.
Bash Special Parameters: The Hidden Power Tools

Bash provides several special parameters that are automatically set by the shell. These parameters can only be referenced - never assigned to - and they provide powerful functionality for script writing.

## The Complete List of Special Parameters

| Parameter | Description |
|-----------|-------------|
| `$0` | Name of the script or shell |
| `$#` | Number of positional parameters |
| `$*` | All parameters as a single string |
| `$@` | All parameters as separate words |
| `$?` | Exit status of last command |
| `$$` | Process ID of current shell |
| `$!` | Process ID of last background command |
| `$-` | Current shell options |
| `$_` | Last argument of previous command |

## Deep Dive into `$*` vs `$@`

These two parameters are the most confusing for beginners, but understanding their difference is crucial.

### Basic Behavior (Unquoted)
```bash
./script.sh one "two three" four
```

Both `$*` and `$@` behave identically when unquoted:
- `$*` → `one two three four`
- `$@` → `one two three four`

### The Critical Difference (Quoted)

This is where the magic happens:

**`"$*"`** = All parameters as **one single string**
```bash
# Arguments: one "two three" four
# "$*" → "one two three four"
```

**`"$@"`** = Each parameter as **separate words**
```bash
# Arguments: one "two three" four
# "$@" → "one" "two three" "four"
```

## Practical Demonstration

### The Word Splitting Problem
Let's see what happens with a script that creates files:

```bash
#!/bin/bash
# test.sh
touch $*
```

**Running:** `./test.sh "file one.txt" "file two.txt"`

**Result:** Creates **4 files** instead of 2:
- `file`
- `one.txt`
- `file`
- `two.txt`

**Why?** Because `$*` without quotes causes word splitting.

### The Solution: Quoted `$@`
```bash
#!/bin/bash
# test.sh
touch "$@"
```

**Running:** `./test.sh "file one.txt" "file two.txt"`

**Result:** Creates **2 files** correctly:
- `file one.txt`
- `file two.txt`

## Real-World Examples

### Example 1: Processing All Arguments
```bash
#!/bin/bash
# process_files.sh

echo "Processing $# files..."

for file in "$@"; do
    echo "Processing: $file"
    # Do something with each file
    wc -l "$file"
done
```

**Why `$@`?** Each filename might contain spaces.

### Example 2: Building a Command String
```bash
#!/bin/bash
# backup.sh

# When you need all arguments as one string
tar -czf backup.tar.gz "$*"
```

**Why `$*`?** When you want to treat all arguments as a single entity.

## The `$_` Parameter

This is often overlooked but incredibly useful:

```bash
#!/bin/bash
# Shows the last argument of the previous command

ls -la /etc/passwd
echo "Last argument was: $_"  # Output: /etc/passwd

cd /var/log
echo "Last argument was: $_"  # Output: /var/log
```

## The `$?` Parameter: Exit Status

### Understanding Exit Codes
```bash
#!/bin/bash
# Check if a file exists

if [ -f "$1" ]; then
    echo "File exists"
else
    echo "File does not exist"
fi

echo "Exit status: $?"  # 0 for success, non-zero for failure
```

### Practical Usage
```bash
#!/bin/bash
# Validate and use exit status

grep "error" logfile.txt
if [ $? -eq 0 ]; then
    echo "Errors found!"
else
    echo "No errors"
fi
```

## The `$$` and `$!` Parameters

### Process IDs
```bash
#!/bin/bash
echo "My PID: $$"

# Start a background process
sleep 60 &
echo "Background PID: $!"

# Kill the background process
kill $!
```

## Advanced Usage Patterns

### Pattern 1: Safe Argument Iteration
```bash
#!/bin/bash
# Always use "$@" for safe iteration

for arg in "$@"; do
    echo "Argument: $arg"
done
```

### Pattern 2: Shift and Process
```bash
#!/bin/bash
# Process arguments one by one

while [ $# -gt 0 ]; do
    case "$1" in
        --file)
            FILE="$2"
            shift 2
            ;;
        --verbose)
            VERBOSE=1
            shift
            ;;
        *)
            echo "Unknown: $1"
            shift
            ;;
    esac
done
```

### Pattern 3: Default Values
```bash
#!/bin/bash
# Provide defaults for missing arguments

FILENAME="${1:-default.txt}"
LINES="${2:-10}"

echo "Processing $FILENAME for $LINES lines"
```

## Common Mistakes to Avoid

### ❌ Wrong: Unquoted `$@` in loops
```bash
for file in $@; do  # Breaks on spaces
    process "$file"
done
```

### ✅ Correct: Quoted `$@`
```bash
for file in "$@"; do
    process "$file"
done
```

### ❌ Wrong: Using `$*` when you need individual args
```bash
echo "$*"  # "one two three" - one string
```

### ✅ Correct: Using `$@` for individual args
```bash
for arg in "$@"; do  # "one", "two", "three" - separate
    echo "$arg"
done
```

## Summary: When to Use What

| Scenario | Use | Why |
|----------|-----|-----|
| **Looping through arguments** | `"$@"` | Preserves spaces in each argument |
| **Counting arguments** | `$#` | Simple count |
| **Getting all as one string** | `"$*"` | Treats all as single entity |
| **Checking success/failure** | `$?` | Exit status |
| **Getting script name** | `$0` | Script invocation name |
| **Process management** | `$$`, `$!` | PIDs for scripting |

## Best Practices

1. **Always quote `$@`** in loops and arrays
2. **Use `$*` only** when you explicitly want one string
3. **Check `$?`** after critical commands
4. **Use `$#`** to validate argument count
5. **Document your script** with `$0` in error messages

## What's Next?

Now that you've mastered special parameters, you're ready to explore **shell expansions** - a powerful feature that lets you generate lists and patterns dynamically!
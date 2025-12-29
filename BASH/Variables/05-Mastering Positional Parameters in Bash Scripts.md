Mastering Positional Parameters in Bash Scripts

Positional parameters are the backbone of command-line arguments in Bash scripting. They allow you to pass data to your scripts when you execute them, making your scripts dynamic and reusable. Let's dive deep into how they work and how to use them effectively.

## What Are Positional Parameters?

Positional parameters are **predefined variables** that store the arguments passed to a script. They are referenced by numbers rather than names, starting from `$1` for the first argument.

### The Three Types of Parameters

1. **Named Parameters (Variables)**: `$my_variable`
2. **Positional Parameters**: `$1`, `$2`, `$3`, etc.
3. **Special Parameters**: `$*`, `$@`, `$?`, `$$`, etc.

## Basic Positional Parameters

### The Script Name (`$0`)
```bash
#!/bin/bash
echo "Script name: $0"
```

**Usage:**
```bash
./myscript.sh
# Output: Script name: ./myscript.sh
```

### The Argument Variables (`$1`, `$2`, `$3`, ...)
```bash
#!/bin/bash
echo "First argument: $1"
echo "Second argument: $2"
echo "Third argument: $3"
```

**Usage:**
```bash
./myscript.sh filename.txt directory1 192.168.1.1
# Output:
# First argument: filename.txt
# Second argument: directory1
# Third argument: 192.168.1.1
```

## Important Syntax Rules

### Curly Braces for Numbers ≥ 10
```bash
# For arguments 1-9:
echo $1 $2 $3 $4 $5 $6 $7 $8 $9

# For argument 10 and beyond:
echo ${10} ${11} ${12}
```

**Why?** `$10` would be interpreted as `$1` followed by `0`, not the 10th argument.

### Alternative Syntax (Curly Braces Optional)
```bash
# Both work for arguments 1-9:
echo $1
echo ${1}

# But curly braces are clearer:
echo ${1}file.txt  # Results in: valuefile.txt
echo $1file.txt    # Results in: valuefile.txt (but less clear)
```

## Practical Examples

### Example 1: Simple Argument Display
```bash
#!/bin/bash
echo "Script: $0"
echo "Argument 1: $1"
echo "Argument 2: $2"
echo "Argument 3: $3"
```

**Run without arguments:**
```bash
./example.sh
# Script: ./example.sh
# Argument 1: 
# Argument 2: 
# Argument 3: 
```

**Run with arguments:**
```bash
./example.sh A B C
# Script: ./example.sh
# Argument 1: A
# Argument 2: B
# Argument 3: C
```

### Example 2: File Display and Compression Script
```bash
#!/bin/bash
# display_and_compress.sh

echo "Displaying contents of: $1"
sleep 2
cat "$1"

echo -e "\nCompressing $1..."
sleep 2
tar -czf "${1}.tar.gz" "$1"

echo "Created: ${1}.tar.gz"
```

**Usage:**
```bash
./display_and_compress.sh important.txt
```

## Providing Default Values

What happens when an argument is missing? You can provide defaults to prevent errors.

### Syntax for Defaults
```bash
${1:-default_value}
```

**The colon (`:`) means:** "If the parameter is unset or empty, use the default."

### Example with Defaults
```bash
#!/bin/bash
# backup.sh

# If no directory provided, default to /tmp
BACKUP_DIR="${1:-/tmp}"

echo "Backing up to: $BACKUP_DIR"
# Continue with backup logic...
```

**Usage:**
```bash
./backup.sh          # Uses /tmp
./backup.sh /home    # Uses /home
```

### More Default Examples
```bash
#!/bin/bash
# file_processor.sh

INPUT_FILE="${1:-input.txt}"
OUTPUT_FILE="${2:-output.txt}"
LINES="${3:-10}"

echo "Processing $INPUT_FILE"
echo "Writing to $OUTPUT_FILE"
echo "Processing $LINES lines"
```

## Counting Arguments

### Using `$#`
The `$#` special parameter holds the number of arguments:

```bash
#!/bin/bash
if [ $# -eq 0 ]; then
    echo "No arguments provided"
    exit 1
fi

echo "You provided $# arguments"
```

## Iterating Over All Arguments

### Using `$@` (Recommended)
```bash
#!/bin/bash
echo "Processing $# files:"

for file in "$@"; do
    echo "  - $file"
done
```

### Using `$*`
```bash
#!/bin/bash
# This treats all arguments as a single string
echo "All arguments: $*"
```

**Key Difference:**
- `"$@"` = Each argument as separate quoted string
- `"$*"` = All arguments as single string

## Real-World Use Cases

### 1. Command-Line Tool
```bash
#!/bin/bash
# greet.sh

NAME="${1:-World}"
COUNT="${2:-1}"

for ((i=1; i<=COUNT; i++)); do
    echo "Hello, $NAME!"
done
```

**Usage:**
```bash
./greet.sh                    # Hello, World! (once)
./greet.sh Alice 3            # Hello, Alice! (three times)
```

### 2. File Validator
```bash
#!/bin/bash
# validate_file.sh

if [ $# -ne 1 ]; then
    echo "Usage: $0 <filename>"
    exit 1
fi

if [ ! -f "$1" ]; then
    echo "Error: $1 is not a file"
    exit 1
fi

echo "File $1 exists and is readable"
```

### 3. Batch Processor
```bash
#!/bin/bash
# batch_convert.sh

for input_file in "$@"; do
    output_file="${input_file%.*}.mp3"
    echo "Converting $input_file to $output_file"
    # ffmpeg -i "$input_file" "$output_file"
done
```

**Usage:**
```bash
./batch_convert.sh song1.wav song2.wav song3.wav
```

## Special Positional Parameters

| Parameter | Description |
|-----------|-------------|
| `$0` | Script name |
| `$1` - `$9` | Arguments 1-9 |
| `${10}` - `${N}` | Arguments 10+ |
| `$#` | Number of arguments |
| `$*` | All arguments as single string |
| `$@` | All arguments as separate words |
| `$?` | Exit status of last command |
| `$$` | Current process ID |
| `$!` | Last background process ID |

## Best Practices

1. **Always quote variables**: `"$1"` not `$1`
2. **Check argument count**: `if [ $# -lt 2 ]; then ...`
3. **Provide usage messages**: Help users understand how to use your script
4. **Use defaults wisely**: Make scripts flexible but not unpredictable
5. **Validate input**: Check if files exist, values are numeric, etc.
6. **Handle errors gracefully**: Exit with meaningful error messages

## Summary

- **Positional parameters** are accessed via `$1`, `$2`, `$3`, etc.
- **$0** contains the script name
- **$#** contains the number of arguments
- **$@** is the preferred way to reference all arguments
- **Curly braces** are required for arguments ≥ 10: `${10}`
- **Defaults** can be provided: `${1:-default}`
- **Always quote** your variables to prevent word splitting

## What's Next?

Now that you understand both interactive input and command-line arguments, you're ready to combine these skills in **real-world projects** that put everything together!
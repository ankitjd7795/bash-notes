Creating Your First Shell Script and Understanding PATH

Let's dive into hands-on Bash scripting by creating your very first script! This practical guide will walk you through the entire process from creation to execution, plus the crucial concept of the PATH variable.

## Setting Up Your Workspace

### Creating a Scripts Directory
First, create a dedicated directory for your scripts in your home directory:
```bash
mkdir ~/scripts
```

**Alternative naming**: Many users prefer `~/bin` for their personal scripts, which is also a common convention.

### Navigate to Your Scripts Directory
```bash
cd ~/scripts
```

## Creating Your First Script

### Using a Text Editor
Create your script file using any text editor (vim, nano, gedit, etc.):
```bash
vim first_script.sh
```

**Important Notes on Filenames**:
- **Avoid spaces** in filenames - use underscores instead
- **Extension convention**: `.sh` extension is common but optional
- **Linux tradition**: Extensions aren't strictly necessary for executables

### Writing the Script Content
Enter these commands exactly as you would type them in the terminal:

```bash
#!/bin/bash

# Create a new directory
mkdir -p drun

# Create a file with content
echo "hello bash world" > drun/file.txt

# List current directory contents
ls -la .

# Display the file contents
cat drun/file.txt
```

**Explanation**:
- `mkdir -p drun`: Creates directory `drun` (with `-p` to avoid errors if it exists)
- `echo "hello bash world" > drun/file.txt`: Writes text to a file
- `ls -la .`: Lists current directory details
- `cat drun/file.txt`: Displays file contents

## Making Your Script Executable

Before you can run your script, you need to give it execute permissions:

### Option 1: Owner Only (Recommended for personal scripts)
```bash
chmod 700 first_script.sh
```

### Option 2: All Users
```bash
chmod +x first_script.sh
```

## Running Your Script

### Method 1: Using Relative Path (Current Directory)
```bash
./first_script.sh
```

The `./` tells the shell to look in the current directory.

### Method 2: Using Absolute Path (From Any Directory)
```bash
/home/student/scripts/first_script.sh
```

### Method 3: Just the Script Name (After PATH Configuration)
Once you configure your PATH (explained below), you can simply run:
```bash
first_script.sh
```

## Understanding the PATH Variable

### What is PATH?
The `PATH` variable is an environment variable that tells the shell **where to look for executable files**. When you type a command, the shell searches through directories listed in `PATH` in order.

### Viewing Your Current PATH
```bash
echo $PATH
```

You'll see something like:
```
/usr/local/bin:/usr/bin:/bin:/usr/local/games:/usr/games
```

### Why Running Just the Script Name Fails Initially
When you type `first_script.sh`, the shell searches for it in the PATH directories. Since your `~/scripts` directory isn't in PATH, you get "command not found."

## Adding Your Scripts Directory to PATH

### Temporary Addition (Current Session Only)
```bash
export PATH=$PATH:~/scripts
```

### Permanent Addition (Persists After Reboot)

1. **Open your `.bashrc` file**:
   ```bash
   nano ~/.bashrc
   ```

2. **Add this line at the end**:
   ```bash
   export PATH="$PATH:~/scripts"
   ```

3. **Save and exit** (Ctrl+X, then Y, then Enter in nano)

4. **Reload the configuration**:
   ```bash
   source ~/.bashrc
   ```

   Or simply open a new terminal.

### Verify the Change
```bash
echo $PATH
```

You should now see `~/scripts` at the end of the PATH.

## Important Security Consideration

### PATH Order Matters
The shell searches directories in the order they appear in PATH. If you have a script named `ls` in your scripts directory, it **won't** override the system's `ls` command because `/bin` appears before `~/scripts` in the PATH.

**Best Practice**: Avoid naming your scripts the same as common system commands to prevent confusion.

## Troubleshooting Common Issues

### "Command Not Found" Errors
- **Check if script is executable**: `ls -la first_script.sh`
- **Verify PATH includes script directory**: `echo $PATH`
- **Reload .bashrc**: `source ~/.bashrc`

### Permission Denied
- **Add execute permission**: `chmod +x first_script.sh`

### Script Runs But Commands Fail
- **Check command syntax**: Ensure commands work when typed directly in terminal
- **Verify file paths**: Use absolute paths or relative paths correctly

## Key Takeaways

- **Scripts are just text files** containing commands you'd normally type
- **Permissions are required**: Scripts need execute permission (`chmod +x`)
- **PATH determines accessibility**: The shell only finds commands in PATH directories
- **Two execution methods**: Relative path (`./script.sh`) or absolute path (`/full/path/script.sh`)
- **PATH modifications**: Can be temporary (current session) or permanent (via `.bashrc`)
- **Security matters**: Be mindful of PATH order and script naming

## Next Steps

Now that you've created your first script, you can:
- Add more complex logic (conditionals, loops, functions)
- Create aliases for your most-used scripts
- Schedule scripts to run automatically with cron
- Share scripts with other users

Your journey into Bash automation has officially begun!
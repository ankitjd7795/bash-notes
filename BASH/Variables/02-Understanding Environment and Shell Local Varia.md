Understanding Environment and Shell Local Variables

In Bash scripting, variables come in two distinct types: **environment variables** and **shell local variables**. Understanding the difference between them is crucial for writing effective scripts and managing your shell environment.

## What Are Environment and Shell Local Variables?

When you start a terminal session, Bash initializes with a collection of predefined variables that customize your system's behavior. These fall into two categories:

### Environment Variables
- **Scope**: Available to the current shell **and** all child processes
- **Lifetime**: Persist until the session ends
- **Purpose**: Pass configuration data to programs
- **Also known as**: Exported variables

### Shell Local Variables
- **Scope**: Available **only** to the current shell
- **Lifetime**: Exist only while the shell is running
- **Purpose**: Store temporary data for the current session
- **Not inherited**: Child processes don't see them

## Key Differences at a Glance

| Feature | Environment Variables | Shell Local Variables |
|---------|----------------------|----------------------|
| **Visibility** | Current shell + child processes | Current shell only |
| **Creation** | Using `export` | Direct assignment |
| **Inheritance** | Yes, by child processes | No |
| **Examples** | `PATH`, `HOME`, `USER` | `BASH_VERSION`, `PPID` |

## Viewing Variables

### Environment Variables Only
```bash
env
# or
printenv
```

### All Variables (Environment + Local)
```bash
set
```

### Searching for Specific Variables
```bash
env | grep PATH
set | grep bash
```

### Getting a Single Variable's Value
```bash
printenv HOME
# or
echo $HOME
```

## Practical Examples

### Example 1: The PATH Variable
The `PATH` variable is a perfect example of an environment variable:
```bash
echo $PATH
# Output: /usr/local/bin:/usr/bin:/bin:/usr/sbin:/sbin
```

This variable tells the shell where to look for executable commands. It's inherited by every program you run.

### Example 2: User-Specific Information
```bash
echo "User: $USER"
echo "Home: $HOME"
echo "Shell: $SHELL"
```

These are all environment variables set when you log in.

### Example 3: Shell Local Variables
```bash
# These are local to your current shell
echo $BASH_VERSION
echo $PPID  # Parent process ID
```

Try running `env | grep BASH_VERSION` - you won't find it because it's not exported.

## Creating Variables

### Creating Shell Local Variables
```bash
my_var="hello"
abc=123
```

These variables exist only in your current shell.

### Creating Environment Variables
```bash
export MY_VAR="hello"
# or in two steps:
MY_VAR="hello"
export MY_VAR
```

Now `MY_VAR` is available to any child processes you spawn.

## Making Variables Persistent

### User-Specific Variables
Add to your shell configuration files:

**`~/.bashrc`** (for interactive shells):
```bash
export MY_CUSTOM_VAR="value"
```

**`~/.bash_profile`** (for login shells):
```bash
export MY_CUSTOM_VAR="value"
```

### System-Wide Variables
For all users, edit system files (requires root):

**`/etc/environment`**:
```
MY_SYSTEM_VAR="value"
```

**`/etc/profile` or `/etc/bash.bashrc`**:
```bash
export MY_SYSTEM_VAR="value"
```

## Real-World Use Cases

### 1. Adding Directories to PATH
```bash
# Add your scripts directory to PATH
export PATH="$PATH:$HOME/scripts"
```

### 2. Setting Default Editors
```bash
export EDITOR=vim
export VISUAL=vim
```

### 3. Custom Application Settings
```bash
export DATABASE_URL="postgresql://localhost/mydb"
export API_KEY="your-secret-key"
```

## Important Commands

### View All Environment Variables
```bash
env
printenv
```

### View a Specific Variable
```bash
printenv HOME
echo $HOME
```

### View All Variables (including local)
```bash
set
```

### Clean Up set Output
```bash
set -o posix  # Removes functions from output
set           # Now shows only variables
```

### Remove a Variable
```bash
unset MY_VAR      # Removes from current shell
export -n MY_VAR  # Removes from environment (keeps it local)
```

## Common Environment Variables

| Variable | Purpose |
|----------|---------|
| `PATH` | Directories to search for commands |
| `HOME` | User's home directory |
| `USER` | Current username |
| `SHELL` | Default shell |
| `PWD` | Current working directory |
| `LANG` | Language/locale settings |
| `EDITOR` | Default text editor |
| `TERM` | Terminal type |

## Best Practices

1. **Use UPPERCASE** for environment variables by convention
2. **Export only what's needed** - don't pollute the environment
3. **Document your variables** - add comments in config files
4. **Check before setting** - avoid overwriting existing variables
5. **Use descriptive names** - `MYAPP_DB_HOST` not `DBH`

## Summary

- **Environment variables** = Global, inherited, persistent
- **Shell local variables** = Local, temporary, isolated
- **`export`** = The key to making variables environment-wide
- **`env`/`printenv`** = View environment variables
- **`set`** = View everything
- **Configuration files** = Make variables persistent across sessions

## What's Next?

Now that you understand variables, you're ready to learn about **getting user input** - an essential skill for creating interactive scripts!
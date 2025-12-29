Mastering Different Ways to Run Bash Scripts

Understanding the various methods to execute Bash scripts is crucial for effective shell scripting. Each approach has its specific use cases, permissions requirements, and behavioral differences. Let's explore the most common and practical ways to run your scripts.

## The Standard Method: Execution Permission

The most common and recommended approach involves two simple steps:

### Step 1: Set Execution Permissions
```bash
chmod +x scriptname.sh
```

### Step 2: Execute Using Relative or Absolute Path
- **Relative path** (when in the same directory):
  ```bash
  ./scriptname.sh
  ```
- **Absolute path** (from anywhere):
  ```bash
  /home/user/scripts/scriptname.sh
  ```

### Important Note About PATH
By default, running just the script name (`scriptname.sh`) will result in a "command not found" error. This happens because your shell doesn't search the current directory for executables. You have two solutions:

1. **Use `./` prefix** (recommended for security)
2. **Add script directory to PATH** (for frequently used scripts)

## Method 2: Explicit Interpreter Call

You can bypass execution permissions by explicitly calling the interpreter:

```bash
bash scriptname.sh
python3 scriptname.sh
```

### Key Characteristics:
- **No execution permission required**
- **Overrides shebang directive** - the interpreter you specify takes precedence
- **Useful for testing** different interpreters or debugging

### Example:
```bash
# Remove execution permission
chmod -x scriptname.sh

# Still executable via explicit interpreter
bash scriptname.sh
```

## Method 3: Source Command

The `source` command (or its shorthand `.`) executes scripts in the current shell environment:

```bash
source scriptname.sh
# OR
. scriptname.sh
```

### Critical Differences from Other Methods:

| Aspect | `./script.sh` | `source script.sh` |
|--------|---------------|-------------------|
| **Execution Environment** | New sub-shell | Current shell |
| **Permissions Required** | Yes | No |
| **Variable Scope** | Isolated | Shared with current session |
| **Exit Behavior** | Returns to parent shell | Stays in current shell |

### Practical Implications:
- **Environment setup**: Use `source` for scripts that modify environment variables
- **Function definitions**: Functions defined in sourced scripts become available in current session
- **Configuration files**: `.bashrc`, `.profile` are sourced, not executed

## Choosing the Right Method

### When to Use Each Approach:

**Use `./script.sh` (with execution permission):**
- General-purpose scripts
- Standalone applications
- When you want isolation from current shell
- **This is the recommended default method**

**Use `bash script.sh`:**
- Testing scripts without changing permissions
- Debugging or development
- When you need to override the shebang

**Use `source script.sh`:**
- Loading environment variables
- Defining functions for current session
- Configuration scripts
- When you need the script to affect your current shell

## Security Considerations

Always prefer `./script.sh` over adding `.` to your PATH. This prevents:
- Accidental execution of malicious scripts
- Confusion about which script is running
- Security vulnerabilities from unknown script locations

## Summary of Best Practices

1. **Default choice**: Use `chmod +x` and `./script.sh`
2. **For environment changes**: Use `source` or `. script.sh`
3. **For testing**: Use `bash script.sh` without changing permissions
4. **Avoid**: Adding current directory to PATH for security reasons

## Section 1 Complete! 🎉

Congratulations on completing the first section of your Bash scripting journey! You've learned:

- ✅ The difference between shells and scripts
- ✅ How to create your first shell script
- ✅ Understanding and using shebangs
- ✅ Adding comments for clarity
- ✅ Multiple ways to execute scripts

### What's Next?

You're now ready to dive deeper into more advanced topics:
- **Variables and parameters**
- **Shell expansions**
- **User input handling**
- **And much more!**

These skills will transform you from a beginner into a proficient Bash scripter. Keep practicing, and I'll see you in the next section!
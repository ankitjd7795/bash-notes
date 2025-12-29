Project: Building a Hacker Protection Script

Congratulations on making it this far! You've learned the fundamentals of Bash scripting, and now it's time to put everything together in a practical, real-world project. As a system administrator, you'll often face security challenges, and automation is your best friend.

## The Challenge: Real-Time Attack Detection

When monitoring servers, you'll frequently see authentication failure messages in your logs:

```
Failed password for root from 192.168.1.100 port 22 ssh2
Failed password for admin from 203.0.113.45 port 22 ssh2
```

These indicate brute force attacks where hackers are trying to guess passwords. Each attempt comes with a source IP address that you can block.

## The Solution: Automated IP Blocking

We'll create a script that automatically blocks malicious IP addresses using `iptables`, the Linux firewall tool.

## Project Structure

### Version 1: Interactive Script
```bash
#!/bin/bash
# dropPackets.sh - Interactive IP blocker

# Prompt user for the target
read -p "Enter IP network or domain to drop: " IP

# Inform the user
echo "Blocking connections from $IP"

# Give user time to read
sleep 1

# Execute the iptables command
sudo iptables -I INPUT -s $IP -j DROP

echo "Done. All packets from $IP are now being dropped."
```

### Version 2: Command-Line Argument Script
```bash
#!/bin/bash
# dropPackets_arg.sh - Automated IP blocker

# Check if argument provided
if [ $# -eq 0 ]; then
    echo "Usage: $0 <IP_ADDRESS>"
    exit 1
fi

# Display what we're doing
echo "Blocking connections from $1"

# Give user time to read
sleep 1

# Execute the iptables command
sudo iptables -I INPUT -s $1 -j DROP

echo "Done. All packets from $1 are now being dropped."
```

## Key Concepts Applied

### 1. **Variable Usage**
- `$IP` stores user input in Version 1
- `$1` stores the first argument in Version 2
- `$#` checks if arguments were provided

### 2. **User Input Methods**
- `read -p` for interactive prompts
- Positional parameters for automation

### 3. **Command Execution**
- `sudo` for root privileges
- `iptables` for firewall rules
- `sleep` for user experience

### 4. **Error Handling**
- Checking argument count
- Providing usage instructions

## Testing the Scripts

### Interactive Version
```bash
chmod +x dropPackets.sh
sudo ./dropPackets.sh
# Enter IP network or domain to drop: 1.1.1.1
```

### Argument Version
```bash
chmod +x dropPackets_arg.sh
sudo ./dropPackets_arg.sh 8.8.8.8
```

### Verification
```bash
# Check if rule was added
sudo iptables -L INPUT -v -n

# Test the block
ping 1.1.1.1  # Should fail
```

## Real-World Application

### Scenario: SSH Attack Monitoring
```bash
#!/bin/bash
# monitor_ssh.sh - Detect and block SSH attacks

# Get failed attempts from logs
FAILED_IPS=$(grep "Failed password" /var/log/auth.log | awk '{print $11}' | sort | uniq -c | sort -nr | head -10)

echo "Top 10 SSH attack sources:"
echo "$FAILED_IPS"

# Ask to block
read -p "Block top attacker? (y/n): " choice

if [ "$choice" = "y" ]; then
    ATTACKER_IP=$(echo "$FAILED_IPS" | head -1 | awk '{print $2}')
    sudo iptables -I INPUT -s $ATTACKER_IP -j DROP
    echo "Blocked $ATTACKER_IP"
fi
```

## Advanced Features to Consider

### 1. **Input Validation**
```bash
# Validate IP format
if [[ ! $IP =~ ^[0-9]+\.[0-9]+\.[0-9]+\.[0-9]+$ ]]; then
    echo "Error: Invalid IP address format"
    exit 1
fi
```

### 2. **Multiple IP Support**
```bash
# Block multiple IPs
for ip in "$@"; do
    echo "Blocking $ip"
    sudo iptables -I INPUT -s $ip -j DROP
done
```

### 3. **Unblock Functionality**
```bash
# Remove blocking rule
sudo iptables -D INPUT -s $IP -j DROP
```

### 4. **Logging**
```bash
# Log blocks to file
echo "$(date): Blocked $IP" >> /var/log/blocked_ips.log
```

## Security Considerations

⚠️ **Important Notes:**
- **Root privileges required**: `iptables` needs sudo
- **Be careful**: Wrong rules can lock you out
- **Test first**: Use `iptables -L` to verify
- **Backup rules**: `iptables-save > backup.txt`
- **Emergency access**: Keep console access available

## Best Practices for Production Scripts

1. **Always validate input**
2. **Provide clear usage messages**
3. **Log all actions**
4. **Add confirmation prompts**
5. **Include help option**
6. **Handle errors gracefully**
7. **Document your code**

## Summary of Skills Used

✅ **Variables**: Storing user input and IP addresses  
✅ **Positional Parameters**: Command-line arguments  
✅ **User Input**: `read` command with prompts  
✅ **Command Execution**: Running iptables with sudo  
✅ **Control Flow**: If statements and loops  
✅ **Error Handling**: Checking arguments and input  
✅ **Real-world Application**: Security automation  

## Next Steps

You now have a working security tool! Consider expanding it:

1. **Add a menu system** for different operations
2. **Create a whitelist** of trusted IPs
3. **Integrate with fail2ban** for automatic blocking
4. **Add email notifications** when blocking occurs
5. **Create a web interface** for remote management

This project demonstrates how Bash scripting can solve real security problems that system administrators face daily. Keep practicing and building - the best way to learn is by doing!
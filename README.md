# Linux Security Audit

Here’s a list of commands that can help check for potential security issues on a Linux/Ubuntu machine.

````markdown
# Linux Security Audit Commands

This is a collection of commands to help assess the security of your Linux/Ubuntu machine. Use these commands to check for signs of compromise, misconfiguration, and other potential security risks.

---

## 1. **Check for Unauthorized Users or Groups**

### List all users on the system:
```bash
cat /etc/passwd
````

### List all groups on the system:

```bash
cat /etc/group
```

### Check for any suspicious or unauthorized accounts:

```bash
sudo cat /etc/passwd | grep -E 'nologin|false|bin'
```

---

## 2. **Check for Suspicious Processes**

### List all running processes:

```bash
ps aux
```

### Check for processes running as root:

```bash
ps aux | grep root
```

### Check for suspicious or unusual processes (e.g., long-running processes or processes with odd names):

```bash
top
```

---

## 3. **Check Open Ports and Listening Services**

### List open ports and the associated processes:

```bash
sudo netstat -tulnp
```

### Check for listening ports:

```bash
ss -tuln
```

### View active network connections:

```bash
sudo lsof -i -n
```

---

## 4. **Check for Malware and Rootkits**

### Use `chkrootkit` to scan for rootkits:

```bash
sudo chkrootkit
```

### Use `rkhunter` to scan for rootkits:

```bash
sudo rkhunter --check
```

---

## 5. **Check User Sudo Privileges**

### List users with sudo privileges:

```bash
sudo grep -E '^sudo|^wheel' /etc/group
```

### Check for users with unnecessary sudo privileges:

```bash
sudo cat /etc/sudoers
```

---

## 6. **Check for Cron Jobs**

### List all cron jobs for all users:

```bash
sudo crontab -l
sudo ls -la /etc/cron.d/
sudo ls -la /var/spool/cron/crontabs/
```

### Check for suspicious cron jobs:

```bash
grep -i 'hack' /etc/cron* /var/spool/cron/crontabs/
```

---

## 7. **File Integrity Check**

### Use `AIDE` (Advanced Intrusion Detection Environment) for file integrity checking:

```bash
sudo apt-get install aide
sudo aideinit
```

### Check file integrity:

```bash
sudo aide --check
```

---

## 8. **Check System Logs for Suspicious Activity**

### View authentication logs (e.g., failed login attempts):

```bash
sudo less /var/log/auth.log
```

### Check for unusual login attempts:

```bash
sudo grep 'Failed password' /var/log/auth.log
```

### Check the history of `sudo` commands:

```bash
sudo cat /var/log/sudo.log
```

---

## 9. **Check for Suspicious SSH Connections**

### Check for active SSH sessions:

```bash
who
```

### Check SSH logins in the last 24 hours:

```bash
last -i
```

### Check for unauthorized SSH keys in the authorized\_keys file:

```bash
cat ~/.ssh/authorized_keys
```

---

## 10. **Check System Updates**

### Ensure your system is up-to-date:

```bash
sudo apt update
sudo apt upgrade
```

### Check for missing security patches:

```bash
sudo apt-get dist-upgrade
```

---

## 11. **Check File Permissions**

### Check for world-writable files:

```bash
find / -xdev -type f -perm -0002 -exec ls -l {} \;
```

### Check for suid/sgid files:

```bash
find / -type f \( -perm -4000 -o -perm -2000 \) -exec ls -l {} \;
```

---

## 12. **Check Disk Usage for Suspicious Files**

### Check for large or unusual files:

```bash
sudo du -ahx / | sort -rh | head -n 20
```

### Look for hidden files or directories:

```bash
ls -la / | grep '^\.' 
```

---

## 13. **Check for System Services and Daemons**

### List active services and daemons:

```bash
sudo systemctl list-units --type=service --state=running
```

### Check for unexpected or unknown services:

```bash
sudo systemctl list-units --type=service
```

---

## 14. **Check for Known Vulnerabilities**

### Use `lynis` for a full security audit:

```bash
sudo apt-get install lynis
sudo lynis audit system
```

### Use `nmap` for a security scan:

```bash
sudo apt-get install nmap
sudo nmap -sS -sV -O 127.0.0.1
```

---

## 15. **Check for Unused Software Packages**

### List installed packages:

```bash
dpkg --list
```

### List packages that are no longer required:

```bash
sudo apt autoremove
```

---

## Conclusion

This set of commands should provide you with a basic security overview of your system. For more in-depth analysis, consider running full security audits and applying best practices for system hardening.

```

---

This markdown file can serve as a great starting point, but depending on your focus, you can add more tools or refine the suggestions. If you’re integrating any automated scripts or specific tools into your repo, you can expand this list with those commands as well.

What do you think of this structure?
```

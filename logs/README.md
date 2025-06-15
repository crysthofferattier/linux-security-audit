# Log Checks

Log-related commands, specifically for monitoring system logs, authentication logs, and other critical log files.

````markdown
# Log-Based Security Audits for Linux

This section focuses on auditing system logs to check for suspicious activity, failed login attempts, privilege escalation attempts, and other potential signs of compromise. Regularly reviewing logs is essential for detecting unusual behavior on your system.

---

## 1. **Authentication Logs**

Authentication logs can reveal information about login attempts, sudo usage, and potential brute force attacks.

### View the system's authentication logs:
```bash
sudo less /var/log/auth.log
````

### Check for failed login attempts (incorrect passwords, etc.):

```bash
sudo grep 'Failed password' /var/log/auth.log
```

### Check for successful logins:

```bash
sudo grep 'Accepted password' /var/log/auth.log
```

### Check for unusual login attempts or login locations (IP addresses):

```bash
sudo grep 'sshd' /var/log/auth.log
```

### Check for users that failed to authenticate multiple times (possible brute force):

```bash
sudo grep 'Failed password' /var/log/auth.log | awk '{print $11}' | sort | uniq -c | sort -nr
```

### Check for sudo access attempts:

```bash
sudo grep 'sudo' /var/log/auth.log
```

### Check for any authentication-related issues:

```bash
sudo grep 'authentication failure' /var/log/auth.log
```

---

## 2. **Last Logins & History**

### View the last logged-in users:

```bash
last
```

### Check for unusual login times or logins from unexpected IP addresses:

```bash
last -i
```

### Display a detailed log of a specific user’s login attempts:

```bash
lastlog -u <username>
```

### View `~/.bash_history` or `~/.zsh_history` for suspicious commands run by users:

```bash
cat ~/.bash_history
cat ~/.zsh_history
```

---

## 3. **System Logs**

System logs are useful for identifying unexpected system reboots, kernel panics, or other system-level issues.

### View the syslog (general system logs):

```bash
sudo less /var/log/syslog
```

### Check for unusual system events (such as hardware errors or kernel panics):

```bash
sudo grep -i 'error' /var/log/syslog
```

### Check for recent system shutdowns or reboots:

```bash
sudo last -x | grep reboot
```

### Check for any system crashes or kernel panic messages:

```bash
sudo grep -i 'panic' /var/log/syslog
```

---

## 4. **Security Logs**

Security logs often contain details on potential attacks or intrusion attempts.

### View the audit logs:

```bash
sudo less /var/log/audit/audit.log
```

### Check for abnormal security events, such as access violations or file changes:

```bash
sudo grep 'AVC' /var/log/audit/audit.log
```

### Check for unauthorized access to sensitive files:

```bash
sudo grep 'open' /var/log/audit/audit.log
```

### Search for attempts to escalate privileges:

```bash
sudo grep 'execve' /var/log/audit/audit.log
```

---

## 5. **Application Logs**

Monitoring application-specific logs can give insights into potential misconfigurations or abnormal behavior.

### View Apache logs for unauthorized access or errors:

```bash
sudo less /var/log/apache2/access.log
sudo less /var/log/apache2/error.log
```

### View Nginx logs for unauthorized access or errors:

```bash
sudo less /var/log/nginx/access.log
sudo less /var/log/nginx/error.log
```

### Check for database-related logs (e.g., MySQL):

```bash
sudo less /var/log/mysql/error.log
```

---

## 6. **Cron Job Logs**

Cron job logs are important for detecting scheduled tasks that may have been created by attackers or unauthorized users.

### View the cron logs:

```bash
sudo less /var/log/syslog | grep CRON
```

### Check the cron logs for any suspicious jobs:

```bash
sudo grep 'CRON' /var/log/syslog
```

---

## 7. **Kernel Logs**

Kernel logs are essential for monitoring system-level events, particularly around the kernel, hardware, and device drivers.

### View the kernel log:

```bash
sudo less /var/log/kern.log
```

### Search for hardware-related issues or errors:

```bash
sudo grep -i 'error' /var/log/kern.log
```

---

## 8. **Mail Logs**

Mail logs can help detect attempts to abuse email services or send spam from the system.

### View mail logs:

```bash
sudo less /var/log/mail.log
```

### Check for unexpected email sending activities:

```bash
sudo grep 'postfix' /var/log/mail.log
```

---

## 9. **Audit Logs for Sudo Usage**

If an attacker has gained sudo privileges, they might try to execute commands with elevated rights. Checking for suspicious sudo activity is crucial.

### View sudo logs for unauthorized sudo attempts:

```bash
sudo less /var/log/sudo.log
```

### Search for commands run with `sudo`:

```bash
sudo grep 'sudo' /var/log/sudo.log
```

### List users with recent sudo privileges:

```bash
sudo grep 'sudo' /var/log/auth.log | grep -v 'COMMAND='
```

---

## 10. **Logs Related to SSH Access**

SSH access logs are critical for detecting unauthorized SSH login attempts, especially brute force attacks.

### View the SSH login logs:

```bash
sudo less /var/log/auth.log | grep sshd
```

### Look for failed SSH login attempts:

```bash
sudo grep 'Failed password' /var/log/auth.log | grep sshd
```

### Check for SSH brute force attempts:

```bash
sudo grep 'Failed password' /var/log/auth.log | awk '{print $11}' | sort | uniq -c | sort -nr
```

---

## 11. **File Integrity Monitoring Logs**

If you're using file integrity tools like `AIDE` or `OSSEC`, you should check the logs to verify if any changes have been detected.

### View `AIDE` logs:

```bash
sudo less /var/log/aide/aide.log
```

### View `OSSEC` logs:

```bash
sudo less /var/ossec/logs/alerts/alerts.log
```

---

## 12. **System Log Rotation**

Log rotation is essential for ensuring that logs are periodically archived and rotated to prevent them from growing indefinitely.

### Check current log rotation settings:

```bash
sudo less /etc/logrotate.conf
sudo less /etc/logrotate.d/*
```

### Ensure that logs are being rotated properly (check timestamps):

```bash
ls -lh /var/log/
```

---

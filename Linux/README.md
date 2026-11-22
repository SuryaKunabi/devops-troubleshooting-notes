
# 🐧 Linux Troubleshooting – Real Issues & Solutions

This document contains Linux-related troubleshooting scenarios, commands, errors, and solutions encountered during hands-on DevOps practice using Ubuntu, AWS EC2, Ansible, and Linux environments.

---

## 1. SSH Connection Issues

**Problem:**

Unable to connect to a remote Ubuntu server through SSH.

**Common Error:**

```text
ssh: connect to host <server-ip> port 22: Connection timed out
```

**Possible Root Causes:**

- SSH service is not running.
- Port 22 is blocked by firewall or AWS Security Group.
- Incorrect server IP address.
- Network connectivity issue.

**Troubleshooting:**

```bash
# Check SSH service
sudo systemctl status ssh

# Start SSH service
sudo systemctl start ssh

# Enable SSH on startup
sudo systemctl enable ssh

# Check SSH listening port
sudo ss -tulnp | grep :22

# Test connectivity
ping <server-ip>
```

**Solution:**

Verify the server IP, SSH service, firewall rules, and AWS Security Group inbound rule for TCP port 22.

**Learning:** SSH connectivity requires a running SSH service, valid authentication, and network access.

---

## 2. Linux – Permission Denied

**Problem:**

Unable to access or execute files because of insufficient permissions.

**Error:**

```text
Permission denied
```

**Root Cause:**

The current user does not have the required file or directory permissions.

**Troubleshooting:**

```bash
# Check file permissions
ls -l filename

# Check current user
whoami

# Check user groups
groups
```

**Solution:**

```bash
# Add execute permission
chmod +x filename.sh

# Set file permissions
chmod 755 filename

# Change file ownership
sudo chown username:username filename

# Execute with elevated privileges when required
sudo ./filename.sh
```

**Learning:** Linux file permissions control read, write, and execute access for users, groups, and others.

---

## 3. Linux – Package Installation Issues

**Problem:**

Unable to install packages on Ubuntu using APT.

**Common Errors:**

```text
E: Unable to locate package
```

```text
Could not get lock /var/lib/dpkg/lock-frontend
```

**Possible Root Causes:**

- Package lists are outdated.
- Incorrect package name.
- Another package management process is running.
- Network or repository connectivity issues.

**Troubleshooting:**

```bash
# Update package lists
sudo apt update

# Upgrade installed packages
sudo apt upgrade

# Search for a package
apt search nginx

# Check package manager processes
ps aux | grep apt
```

**Solution:**

Update package lists and verify the package name.

```bash
sudo apt update
sudo apt install nginx
```

If the package manager lock exists, first verify whether another APT or dpkg process is running. Do not delete lock files while a package operation is active.

**Learning:** APT uses package repositories and package management locks to maintain system consistency.

---

## 4. Linux – Nginx Service Failed to Start

**Problem:**

Nginx service failed to start while configuring a web server on Ubuntu.

**Troubleshooting:**

```bash
# Check Nginx status
sudo systemctl status nginx

# Check Nginx configuration
sudo nginx -t

# View service logs
sudo journalctl -u nginx -xe

# Restart Nginx
sudo systemctl restart nginx
```

**Possible Root Causes:**

- Incorrect Nginx configuration.
- Port 80 already in use.
- Missing configuration files.
- File permission problems.

**Solution:**

Identify the specific error using `nginx -t` and service logs, correct the configuration, and restart Nginx.

**Learning:** Always validate configuration files before restarting production services.

---

## 5. Linux – Port Already in Use

**Problem:**

Unable to start an application because another process is already using the required port.

**Common Error:**

```text
Address already in use
```

**Troubleshooting:**

```bash
# Find processes using port 80
sudo lsof -i :80

# Check listening ports
sudo ss -tulnp

# Find process ID
sudo fuser -n tcp 80
```

**Solution:**

Identify the process and determine whether it can safely be stopped.

```bash
sudo systemctl stop nginx
```

**Learning:** Only stop processes after identifying what is using the port and confirming it is safe to do so.

---

## 6. Linux – Disk Space Full

**Problem:**

Applications or services may fail because the Linux filesystem has insufficient available storage.

**Common Error:**

```text
No space left on device
```

**Troubleshooting:**

```bash
# Check filesystem usage
df -h

# Check directory sizes
du -sh *

# Check inode usage
df -i

# Find large files
sudo du -ah /var | sort -rh | head -20
```

**Solution:**

Identify unnecessary files, old logs, or unused resources before safely cleaning them.

For example:

```bash
# Clean APT cache
sudo apt clean

# Check journal log size
sudo journalctl --disk-usage
```

**Learning:** Disk space and inode usage are both important when troubleshooting Linux storage problems.

---

## 7. Linux – Systemd Service Management

**Problem:**

A service does not start or remain active after system reboot.

**Troubleshooting:**

```bash
# Check service status
sudo systemctl status nginx

# Start service
sudo systemctl start nginx

# Stop service
sudo systemctl stop nginx

# Restart service
sudo systemctl restart nginx

# Enable service at boot
sudo systemctl enable nginx

# View service logs
sudo journalctl -u nginx -f
```

**Solution:**

Check service logs, configuration, dependencies, and startup settings. Enable the service if it should automatically start at boot.

**Learning:** Systemd manages Linux services and their startup behavior.

---

## 8. Linux – SSH Authentication and Private Key Permission

**Problem:**

SSH connection fails when using a private key to connect to an Ubuntu server.

**Common Error:**

```text
WARNING: UNPROTECTED PRIVATE KEY FILE!
Permissions for 'key.pem' are too open.
```

**Root Cause:**

SSH refuses to use a private key that is accessible to unauthorized users.

**Solution:**

On Linux:

```bash
chmod 400 key.pem
```

On Windows PowerShell, adjust the file's ACL permissions so that only the intended account has access.

**Learning:** SSH private keys must be protected with appropriate filesystem permissions.

---

## 9. Linux – Process Management and Troubleshooting

**Problem:**

An application or process consumes excessive system resources or becomes unresponsive.

**Troubleshooting:**

```bash
# Display running processes
ps aux

# Monitor CPU and memory
top

# Check process by name
pgrep nginx

# Display process details
ps -p <PID> -f

# Gracefully terminate a process
kill <PID>

# Force termination only if necessary
kill -9 <PID>
```

**Learning:** Process management helps identify resource-intensive applications and investigate system performance problems.

---

## Useful Linux Commands Reference

| Purpose | Command |
|---|---|
| Current user | `whoami` |
| Current directory | `pwd` |
| List files | `ls -la` |
| Change directory | `cd` |
| Create directory | `mkdir` |
| File permissions | `chmod` |
| File ownership | `chown` |
| Disk usage | `df -h` |
| Memory usage | `free -h` |
| Process list | `ps aux` |
| Network interfaces | `ip addr` |
| Listening ports | `ss -tulnp` |
| Service status | `systemctl status` |
| System logs | `journalctl` |
| OS information | `cat /etc/os-release` |

---

## Key Learnings

- Linux command-line administration.
- SSH connectivity and authentication.
- File permissions and ownership.
- APT package management.
- Nginx service management.
- Process and port troubleshooting.
- Disk and memory monitoring.
- Systemd service administration.
- Linux troubleshooting for AWS and Ansible environments.

---

**Learn | Execute | Troubleshoot | Document | Improve**

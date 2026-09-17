 Secure EC2 Bastion & User Access Management
 Project Overview
This project demonstrates the deployment and security hardening of an AWS EC2 instance (Bastion Host). It covers creating a custom administrative user, configuring SSH key-based authentication, and disabling direct root login via SSH to enforce the principle of least privilege and strict access control.
 Tech Stack & Prerequisites
Cloud Provider: Amazon Web Services (AWS)
Compute Service: AWS EC2 (Ubuntu 24.04 LTS)
Management Tool: AWS CloudShell
Protocols: SSH, Public/Private Key Encryption
 Step-by-Step Implementation
1. User Creation & Privilege Escalation
Provisioned a custom system user `hassan` to replace shared default credentials:
```bash
sudo adduser hassan
```
Granted administrative privileges by adding the user to the sudo group:
```bash
sudo usermod -aG sudo hassan
```
2. SSH Key Configuration & Security Hardening
Set up the `.ssh` directory with restricted file permissions (700):
```bash
mkdir -p ~/.ssh && chmod 700 ~/.ssh
```
Copied authorized SSH keys from the default `ubuntu` account and transferred ownership:
```bash
sudo cp /home/ubuntu/.ssh/authorized_keys ~/.ssh/
sudo chown -R hassan:hassan ~/.ssh
chmod 600 ~/.ssh/authorized_keys
```
3. SSH Service Hardening (sshd_config)
Hardened the OpenSSH daemon configuration to reject direct root access over SSH.
Modified `/etc/ssh/sshd_config`:
```
PermitRootLogin no
```
Restarted the SSH daemon to apply security policies:
```bash
sudo systemctl restart ssh
```
📸 Verification & Screenshots
Hardening Step	Description	Screenshot
User & Key Setup	Successful key migration & ownership under `hassan`	![SSH Key Setup](screenshots/ssh-setup.png)
SSH Configuration	Hardened `sshd_config` with `PermitRootLogin no`	![Root Login Disabled](screenshots/sshd-config.png)
  Security Best Practices Applied
  No Direct Root SSH Login: Reduced attack surface by blocking direct root SSH access.
  Key-Based Authentication: Eliminated reliance on passwords for SSH access.
  Strict File Permissions: Applied strict Unix file permissions (700 for `.ssh` and 600 for `authorized_keys`).
  Least Privilege Principle: Individualized user account (`hassan`) with tracked sudo access.

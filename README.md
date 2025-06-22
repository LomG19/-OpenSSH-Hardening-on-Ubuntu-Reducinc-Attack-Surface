# 🛡️ OpenSSH Hardening on Ubuntu – Reducing Attack Surface  
*By Mugisha Loic *

## Project Summary
This project documents the process of securing OpenSSH on Ubuntu to reduce unauthorized access risk and align with cybersecurity best practices

## Objective  
Harden an Ubuntu server's SSH service against:  
- Brute force attacks  
- Credential theft  
- Unauthorized access

  ## Tools & Environment

- Ubuntu 24.04 LTS (Virtual Machine)
- OpenSSH Server
- Google Authenticator (PAM module)
- SSH client (on physical host)
- VMWare Fusion

##  Key Security Features  
- Disabled password authentication  
- Enforced SSH key-based login  
- Added Google Authenticator MFA
- Change default SSH port from 22 to 24214
- Updated UFW firewall rules
- Set the maximum authorized login tries to 3 within 10 seconds
- Disabled root login
- Fixed `sshd_config` syntax errors  

## Step-by-Step Guide  

### 1. Install OpenSSH  
- SSH, or Secure Shell, is an encrypted protocol used to administer and communicate with servers.
    
    ```
    sudo apt install openssh-server -y
    sudo systemctl enable ssh
    
    ```
    
### 2. Generating Ed25519 Key Pairs (Public and Private Key)  
- The key pairs were created on the client machine(in this case, my physical machine). Ed25519 is fast, modern, and secure.
  
    ```
    ssh-keygen
    ```
    
### 3. Copying the Public Key to the Server 
- The public key will be copied to the Ubuntu server (Virtual Machine).
    
    ```
    ssh-copy-id doe@192.168.93.129
    ```
### 4. Disabling Password Authentication
- Disabling Password authentication for enhanced security and to help prevent brute force attacks. This is done by modifying the SSH daemon's file.
    
    ```
  sudo nano /etc/ssh/sshd_config
    ** Before with password authentication enabled **
    
      PasswordAuthentication yes
    
    ** After with password authentication disabled **
    
     PasswordAuthentication no
    ```
    ### 4. Disable root login 
- Every linux system has a root user; disabling it forces the attacker to guess both a valid non-root username and correct credentials.
    
    ```
      sudo nano /etc/ssh/sshd_config
    ** Before **
    
      PermitRootLogin yes
    
    ** After **
    
     PermitRootLogin no
    ```
    ### 5. Set the maximum authorized login tries to 3
- Brute-force scripts rely on rapid retries, and this will slow it down, as for each 3 failed attempts, the session will be closed, forcing the attacker to reconnect after every 3 attempts.
    
    ```
  sudo nano /etc/ssh/sshd_config
    ** Before **
    
      MaxAuthTries 6
    
    ** After **
    
     MaxAuthTries 3
    ```
    ### 6. Changing the SSH default port
- This can help prevent against automated and unsophisticated attacks that usually only target the 22 port, ensuring cleaner logs.
    
    ```
  sudo nano /etc/ssh/sshd_config
    ** Default Port **
    
      Port 22
    
    ** Custom Port set **
    
     Port 24214
    ```
### 7. Set up Multi-Factor Authentication (MFA)
- Adding MFA provides an additional layer of security.
        Install Google PAM(Pluggable Authentication Module)
        
   ```
        sudo apt install  libpam-google-authenticator
   ```
- Initialize the authentication app for a user
        
```
              google-authenticator

  ```
## ⚠️ Troubleshooting  
- Problem: SSH service fails after config changes.
- Diagnose: sshd -t <- to check  misconfiguration, revealed: "Invalid AuthenticationMethods list at line 132".
- Issue: Duplicate password entry, missing commas.
    ```
          **  before with errors **
            AuthenticationMethods publickey, password, password publickey, keyboard-interactive
          **  After correction to require 2FA (publickey + OTP) **
            AuthenticationMethods publickey keyboard-interactive
[📖 Full detailed Documentation on Notion](https://1xVBQ0.short.gy/Server-Hardening)  

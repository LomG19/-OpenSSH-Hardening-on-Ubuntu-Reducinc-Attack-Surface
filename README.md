# 🛡️ OpenSSH Hardening on Ubuntu – Reducing Attack Surface  
*By Mugisha Loic *

## Objective  
Harden an Ubuntu server's SSH service against:  
- Brute force attacks  
- Credential theft  
- Unauthorized access  

##  Key Security Features  
- Disabled password authentication  
- Enforced SSH key-based login  
- Added **Google Authenticator MFA**  
- Fixed `sshd_config` syntax errors  

## Step-by-Step Guide  

### 1. Install OpenSSH  
- SSH, or Secure Shell, is an encrypted protocol used to administer and communicate with servers.
    
    ```
    sudo apt install openssh-server -y
    sudo systemctl enable ssh
    
    ```
    
### 3. Generating Ed25519 Key Pairs (Public and Private Key)  
- The key pairs were created on the client machine(in this case, my physical machine). Ed25519 is fast, modern, and secure.
  
    ```
    ssh-keygen
    ```
    
### 4. Copying the Public Key to the Server 
- The public key will be copied to the Ubuntu server (Virtual Machine).
    
    ```
    ssh-copy-id doe@192.168.93.129
    ```
### 5. Disabling Password Authentication
- Disabling Password authentication for enhanced security and to help prevent brute force attacks. This is done by modifying the SSH daemon's file.
    
    ```
  sudo nano /etc/ssh/sshd_config

    diff
     PasswordAuthentication no <- initially set to yes, it's changed to NO.
    ```
### 6. Set up Multi-Factor Authentication (MFA)
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
- Diagnose: sshd -t <- to check any typo in the configuration file.

[📖 Full detailed Documentation on Notion](https://1xVBQ0.short.gy/Server-Hardening)  

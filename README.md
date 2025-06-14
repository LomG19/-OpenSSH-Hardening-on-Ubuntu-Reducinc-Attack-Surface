# OpenSSH-Ubunutu-Hardening  
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
- SSH, or secure shell, is an encrypted protocol used to administer and communicate with servers.
    bash
    ```
    sudo apt install openssh-server -y
    sudo systemctl enable ssh
    
    ```
    
### 3. Generating ed25519 Key Pairs (Public and Private Key)  
- The key pairs were created on the client machine(in this case my physical machine). Ed25519 are             fast, modern, secure.
    zsh
    ```
    ssh-keygen
    ```
    
### 4. Copying the Public Key to Server 
- The public key will be copied to the Ubuntu server (Virtual Machine).
    zsh
    ```
    ssh-copy-id doe@192.168.93.129
    ```
### 5. Disabling Password Authentication
- Disabling Password authentication for enhanced security and help prevent brute force attacks. This is done by modifying the SSH daemon's file.
    bash
    ```
  sudo nano /etc/ssh/sshd_config

    diff
     PasswordAuthentication no <- initially set to yes it's changed to NO.
    ```
### 6. Setup Multi-Factor Authentication (MFA)
- Adding MFA provides an additional layer of security.
        Install google PAM(Pluggable Authentication Module)
        bash
        ```
        sudo apt install  libpamp-google-authenticator
  
    ```
- Initialize the authentication app for a user
        bash
```
              google-authenticator

  ```
## ⚠️ Troubleshooting  
    - Problem: SSH service fails after config changes.

[📖 Full detailed Documentation on Notion](https://1xVBQ0.short.gy/Server-Hardening)  

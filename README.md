# OpenSSH-Ubunutu-Hardening  
*By [Mugisha Loic]*

##  Key Security Features  
- Disabled password authentication  
- Enforced SSH key-based login  
- Added **Google Authenticator MFA**  
- Fixed `sshd_config` syntax errors  

## 🛠️ Step-by-Step Guide  

### 1. Install OpenSSH  
    - SSH, or secure shell, is an encrypted protocol used to administer and communicate with servers. 
### 2. Enable SSH Keys
    - SSH keys provide a secure way of logging into a server and are recommended for all users.
### 3. Creating Key Pairs (Public and Private Key)  
    - The key pairs were created on the client machine(in this case my phisical machine).
### 4. Copying the Public Key to Server 
    - The public key will be copied to the Ubuntu server (Virtual Machine).
### 4. Disabling Password Authentication
    - Disabling Password authentication for enhanced security and help prevent brute force attacks.
### 5. Setup Multi-Factor Authentication (MFA)
    - Adding MFA provides an additional layer of security.
## ⚠️ Troubleshooting  
    - Problem: SSH service fails after config changes.

[📖 Full detailed Documentation on Notion](https://1xVBQ0.short.gy/Server-Hardening)  

# Configuring OpenSSH Server-ubuntu

# Introduction

SSH, or secure shell, is an encrypted protocol used to administer and communicate with servers. When working with an Ubuntu server, chances are you will spend most of your time in a terminal session connected to your server through SSH. Here I'll describe some of the configuration settings possible with the OpenSSH server application.

# 1. Installation and Setup

## Installing OpenSSH

To install the OpenSSH server applications on your Ubuntu system, we use this command at a terminal prompt:

![Screenshot 2025-06-10 at 15.10.26.png](Configuring%20OpenSSH%20Server-ubuntu%20210c5a6a6b2180139ce5e0e81968bea0/Screenshot_2025-06-10_at_15.10.26.png)

## Starting and Enabling SSH Service

After installation, follow these steps:

- Enable and start the SSH service:
    
    ![Screenshot 2025-06-10 at 15.18.41.png](Configuring%20OpenSSH%20Server-ubuntu%20210c5a6a6b2180139ce5e0e81968bea0/Screenshot_2025-06-10_at_15.18.41.png)
    
- Verify that SSH is running:
    
    ![Screenshot 2025-06-10 at 15.20.00.png](Configuring%20OpenSSH%20Server-ubuntu%20210c5a6a6b2180139ce5e0e81968bea0/Screenshot_2025-06-10_at_15.20.00.png)
    

# 2. Authentication Configuration

Now that SSH is installed and running, we should configure secure authentication methods. SSH keys are more secure than password authentication, and we can enhance security further with 2FA using tools like Google authenticator.

## Setting Up SSH Keys

SSH keys provide a secure way of logging into a server and are recommended for all users.

### 1. Creating Key Pairs

First, create a key pair on your client machine:

![Screenshot 2025-06-10 at 16.30.34.png](Configuring%20OpenSSH%20Server-ubuntu%20210c5a6a6b2180139ce5e0e81968bea0/Screenshot_2025-06-10_at_16.30.34.png)

### 2. Copying the Public Key to Server

The quickest way to copy your public key to the Ubuntu host is to use the utility called `ssh-copy-id`:

![Screenshot 2025-06-10 at 16.45.31.png](Configuring%20OpenSSH%20Server-ubuntu%20210c5a6a6b2180139ce5e0e81968bea0/Screenshot_2025-06-10_at_16.45.31.png)

### 3. Testing SSH Key Authentication

Try logging in with your SSH key:

![Screenshot 2025-06-10 at 16.58.01.png](Configuring%20OpenSSH%20Server-ubuntu%20210c5a6a6b2180139ce5e0e81968bea0/Screenshot_2025-06-10_at_16.58.01.png)

### 4. Disabling Password Authentication

For enhanced security, disable password-based authentication in the SSH daemon's config file:

![Screenshot 2025-06-10 at 17.07.25.png](Configuring%20OpenSSH%20Server-ubuntu%20210c5a6a6b2180139ce5e0e81968bea0/Screenshot_2025-06-10_at_17.07.25.png)

Modify the configuration by uncommenting PasswordAuthentication and setting it to 'no':

![Screenshot 2025-06-10 at 17.06.47.png](Configuring%20OpenSSH%20Server-ubuntu%20210c5a6a6b2180139ce5e0e81968bea0/Screenshot_2025-06-10_at_17.06.47.png)

# 3. Multi-Factor Authentication (MFA)

While SSH keys are secure, they're still a single factor of authentication. Adding MFA provides an additional layer of security.

## Understanding OATH-TOTP

OATH-TOTP (Open Authentication Time-Based One-Time Password) generates a one-time use password, typically a six-digit number that changes every 30 seconds. We'll implement this alongside SSH key authentication for enhanced security.

## Setting Up Google's PAM

- Install Google's PAM (Pluggable Authentication Module):
    
    ![Screenshot 2025-06-10 at 18.06.34.png](Configuring%20OpenSSH%20Server-ubuntu%20210c5a6a6b2180139ce5e0e81968bea0/Screenshot_2025-06-10_at_18.06.34.png)
    

## Configuring MFA

1. Initialize the authentication app:

![Screenshot 2025-06-10 at 18.37.14.png](Configuring%20OpenSSH%20Server-ubuntu%20210c5a6a6b2180139ce5e0e81968bea0/Screenshot_2025-06-10_at_18.37.14.png)

1. Configure the SSH server for MFA:

Open the configuration file:

![Screenshot 2025-06-10 at 19.10.50.png](Configuring%20OpenSSH%20Server-ubuntu%20210c5a6a6b2180139ce5e0e81968bea0/Screenshot_2025-06-10_at_19.10.50.png)

Add these lines to the bottom of the file:

![Screenshot 2025-06-12 at 13.20.03.png](Configuring%20OpenSSH%20Server-ubuntu%20210c5a6a6b2180139ce5e0e81968bea0/Screenshot_2025-06-12_at_13.20.03.png)

1. Enable keyboard-interactive authentication:

Access the SSH config file:

![Screenshot 2025-06-12 at 13.31.19.png](Configuring%20OpenSSH%20Server-ubuntu%20210c5a6a6b2180139ce5e0e81968bea0/Screenshot_2025-06-12_at_13.31.19.png)

Set KbdInteractiveAuthentication to 'yes':

![Screenshot 2025-06-12 at 13.31.02.png](Configuring%20OpenSSH%20Server-ubuntu%20210c5a6a6b2180139ce5e0e81968bea0/Screenshot_2025-06-12_at_13.31.02.png)

# 4. Troubleshooting

1. **Issue**

After all configuration restarting the ssh service failed:

![Screenshot 2025-06-12 at 13.46.40.png](Configuring%20OpenSSH%20Server-ubuntu%20210c5a6a6b2180139ce5e0e81968bea0/Screenshot_2025-06-12_at_13.46.40.png)

1. **Defining the cause**

Running **`sudo sshd -t`** revealed: 

![June 12 Screenshot from DeepSeek.png](Configuring%20OpenSSH%20Server-ubuntu%20210c5a6a6b2180139ce5e0e81968bea0/June_12_Screenshot_from_DeepSeek.png)

1. **Root Cause**
    
    The error was caused by **invalid syntax** in **`/etc/ssh/sshd_config`** at **line 132**:
    

![Screenshot 2025-06-12 at 13.54.15.png](Configuring%20OpenSSH%20Server-ubuntu%20210c5a6a6b2180139ce5e0e81968bea0/Screenshot_2025-06-12_at_13.54.15.png)

- **Duplicate `password` entry**
- **Missing comma in `password publickey`** (SSH read it as one invalid method)
- **Extra spaces** causing parsing errors
1. **Solution**

Replaced line 32 with the following valid format requiring a key +  MFA:

![Screenshot 2025-06-12 at 15.44.46.png](Configuring%20OpenSSH%20Server-ubuntu%20210c5a6a6b2180139ce5e0e81968bea0/Screenshot_2025-06-12_at_15.44.46.png)

# 5. Conclusion

We should now have SSH key-based authentication configured and running on the server, allowing you to sign in without providing an account password but a verification code instead.
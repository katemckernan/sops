# Set Up SSH Key Authentication on a Linux Server

## Purpose
This document acts as a guide for setting up SSH key authentication on a Linux server for remote administration. It follows security best practices, while maintaining consistency across our organization's standards.

## Audience
This document is intended for system administrators and IT teams involved in provisioning our organization's servers.

## Scope
The SOP covers the processes and guidelines for setting up SSH key authentication on a Linux server, including:
1.	Preparing a Linux server for remote administration
2.	Installing and verifying the SSH service
3.	Generating an SSH key pair on a client system
4.	Transferring the public key to the server
5.	Configuring the Linux user account for key-based access
6.	Connecting to the server using the private key
7.	Verifying that SSH key authentication works
8.	Configuring basic firewall access for SSH
9.	Troubleshooting common connection and permission problems
10.	Applying basic SSH security practices

## Prerequisites
- Local admin credentials
- Target user credentials
- Root privileges on remote server
- Basic terminal navigation
- Network connection to the server
- Internet access if SSH installation necessary
- Known IP address for local and remote server
- Supported RHEL based OS on remote server

## Technical Requirements
- **OpenSSH Client** on client system
- **OpenSSH Service** on remote Linux server
- **DNF Package Manager** on remote server
- **IP configuration** on both systems
- **Firewall configuration** on remote server allowing inbound TCP connections on port 22

## Numbered procedure steps
### Step 1: Prepare a Linux server for remote administration:
Step 1.1: Server login
- Boot up your server running a RHEL based OS
- Enter login credentials with root privileges

![1.1image](https://github.com/katemckernan/sops/blob/518360f751b33d05e0dae43dd277fa739be95ead/Screenshot%202026-07-23%20111832.png)

Step 1.2: Subscription verification and device hardening
- Open terminal and enter the following commands:
```bash
# Verify server is running supported OS
cat/etc/os-release
# Register server for access to updates and security patches
sudo subscription-manager register
# Verify license status
sudo subscription-manager status
# List active repositories
sudo subscription-manager repos --list-enabled
# Perform system wide updates
sudo dnf update -y
```
![1.2image](https://github.com/katemckernan/sops/blob/8644e283ed0d007bf0d8db1f623f500b68cd5d42/Screenshot%202026-07-23%20011411.png)

Step 1.3: Verify server network configuration
```bash
# show network device status
nmcli dev status
# show network interfaces
ip link
# show IP addresses assigned to each interface
ip addr
# show ipv4 routing table
ip route
# ping test
ping -c 6 8.8.8.8
# show hostname
hostnamectl
```
![1.3image](https://github.com/katemckernan/sops/blob/e4e2844595e4002a8a79ac871700d8cf34c8fcdf/Screenshot%202026-07-23%20112854.png)

### Step 2: Install and verify the SSH service:
Step 2.1: Install OpenSSH server daemon on the Linux server
```bash
# install the service
sudo dnf install openssh-server -y
# start service and enable it to start at boot
sudo systemctl enable --now sshd
```
![2.1image](https://github.com/katemckernan/sops/blob/dc6b6541a5719493e2642bb44007d0c8a0022bff/Screenshot%202026-07-23%20113531.png)

Step 2.2: Verify service status
```bash
# check if service is running
sudo systemctl status sshd
# show listening TCP ports
ss -tlnp
```
![2.2image](https://github.com/katemckernan/sops/blob/dc6b6541a5719493e2642bb44007d0c8a0022bff/Screenshot%202026-07-23%20113657.png)

### Step 3: Generate an SSH key pair on a client system:
Step 3.1: Verify network configuration
- Login to the client system
- Open powershell and enter the following commands:
```bash
# Verify NIC, IP address, and hostname
ipconfig /all
```
![3.1](https://github.com/katemckernan/sops/blob/d82838e1b4ebcdabdce7e52660e98bc0624552a7/Screenshot%202026-07-23%20114539.png)

Step 3.2: Generate pair of cryptographic key files
```bash
# generate private key and public key
ssh-keygen
```
![3.2image](https://github.com/katemckernan/sops/blob/8644e283ed0d007bf0d8db1f623f500b68cd5d42/Screenshot%202026-07-23%20035813.png)

### Step 4: Transfer the public key to the server and configure linux user for key based access:
Step 4.1: Transfer public key to server from Windows client
- Enter the following command in powershell:
```bash
# point windows to public key file, open secure tunnel to user, change folder permissions, append public key to /.ssh/authorized_keys file
type $env:USERPROFILE\.ssh\id_ed25519.pub | ssh username@targetserverip "mkdir -p ~/.ssh && chmod 700 ~/.ssh && cat >> ~/.ssh/authorized_keys && chmod 600 ~/.ssh/authorized_keys"
```
![4.1image](https://github.com/katemckernan/sops/blob/8644e283ed0d007bf0d8db1f623f500b68cd5d42/Screenshot%202026-07-23%20042048.png)

> **For Linux clients**
> 
>  Replace previous command with ssh-copy-id
>  ```bash
>  ssh-copy-id username@targetserverip
>  ```

### Step 5: Connect to the server using the private key:
Step 5.1: Connect to remote server from powershell
```bash
# ssh into target system
username@targetserverip
```
![5.1image](https://github.com/katemckernan/sops/blob/3381c54a48505e03e5beae02eb3bc3a181f96ff1/Screenshot%202026-07-23%20115355.png)

Step 5.2: Disable PasswordAuthentication
```bash
# use vi text editor to set the PasswordAuthentication parameter to no
sudo vi /etc/ssh/sshd_config
```
![5.2image](https://github.com/katemckernan/sops/blob/8644e283ed0d007bf0d8db1f623f500b68cd5d42/Screenshot%202026-07-23%20050437.png)

### Step 6: Verify that SSH key authentication works:
Step 6.1: Check logs for public key authentication
- From client powershell:
```bash
# connect using verbose flag and look for authenticated using publickey
ssh -v username@targetserverip
```
![6.1image1](https://github.com/katemckernan/sops/blob/8644e283ed0d007bf0d8db1f623f500b68cd5d42/Screenshot%202026-07-23%20055433.png)

- From remote linux terminal
```bash
# check logs using keyword and look for accepted publickey
sudo grep "publickey" /var/log/secure
```
![6.1image2](https://github.com/katemckernan/sops/blob/8644e283ed0d007bf0d8db1f623f500b68cd5d42/Screenshot%202026-07-23%20061139.png)

### Step 7: Configure basic firewall access for SSH:
Step 7.2: Allow SSH connections permanently
```bash
sudo firewall-cmd --permanent --add-service=ssh
```
![7.2image](https://github.com/katemckernan/sops/blob/9b4aaf9f84b2b810fe028c4dc78084637a0853a9/Screenshot%202026-07-23%20063022.png)

Step 7.3: Reload and verify firewall configuration
```bash
# reload to apply changes
sudo firewall-cmd --reload
# verify ssh
sudo firewall-cmd --list-services
```
![7.3image](https://github.com/katemckernan/sops/blob/9b4aaf9f84b2b810fe028c4dc78084637a0853a9/Screenshot%202026-07-23%20063023.png)

### Troubleshoot common connection and permission problems:
- **If IP address, public key, or remote system changes and you try connecting:**
  - key entry for system no longer valid
  - warning that remote host indentity changed
    - **FIX**
      - verify fingerprint
      - remove key entry from /.ssh/knownhosts file then reconnect system for new entry
      - ```bash
        # optional command way to remove key entry
        ssh-keygen -R remoteservername -f ~/.ssh/known_hosts
        ```
- **If permission denied when accessing folders and files:**
  - permission denied
  - no such file or directory
    - **FIX**
      - verify correct user account
      - check permissions
      - change ownership
      - ```bash
        # check current logged in user
        whoami
        # view permissions for specific item
        ls -l /kates/folders/andfiles
        # change rwx permissions
        sudo chmod 777 file.txt
        # change ownership
        sudo chown newuser /kates/folders/andfiles
        # grant admin privileges by appending user to wheel group
        sudo usermod -aG wheel username
        ```

## Definitions
- **OpenSSH** is an open-source suite of security utilities
- **RHEL** is a linux distribution designed for use in businesses
- **DNF** is the default software package manager for RHEL based linux distributions
- **Repositories** are centralized locations used to store, organize, and manage digital assets
- **Daemons** are computer programs that run in the background
- **Cryptographic key files** are digital files that store a string of random data used by software algorithms to encrypt and decrypt secure information, sign digital identities, and log into servers safely
- **Firewalld** is a zone-based firewall which is a network security system that monitors traffic and takes action based on a set of rules 
  
## Security considerations
- Configure password aging
- Configure complex password for key based authentication
- Backup configuration files
- Prevent root user from crossing network via SSH
- Configure SSH to listen on a different port

## Revision History
| Revision | Description           | Date       | Approved By |
|----------|-----------------------|------------|-------------|
| 1        | Created main document | 2026-07-23 | Kate        |

## References
*[administration](https://www.redhat.com/en/blog/administering-remote-systems)*

*[openssh](https://docs.redhat.com/en/documentation/red_hat_enterprise_linux/7/html/system_administrators_guide/ch-openssh)*

*[firewalld](https://www.redhat.com/en/blog/beginners-guide-firewalld)*

*[security](https://www.redhat.com/en/blog/eight-ways-secure-ssh)*

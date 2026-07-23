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

### Step 2: Install and verify the SSH service:
Step 2.1: Install OpenSSH server daemon on the Linux server
```bash
# install the service
sudo dnf install openssh-server -y
# start service and enable it to start at boot
sudo systemctl enable --now sshd
```
Step 2.2: Verify service status
```bash
# check if service is running
sudo systemctl status sshd
# show listening TCP ports
ss -tlnp
```

### Step 3: Generate an SSH key pair on a client system:
Step 3.1: Verify network configuration
- Login to the client system
- Open powershell and enter the following commands:
```bash
# Verify NIC, IP address, and hostname
ipconfig /all
```
Step 3.2: Generate pair of cryptographic key files
```bash
# generate private key and public key
ssh-keygen
```

### Step 4: Transfer the public key to the server and configure linux user for key based access:
Step 4.1: Transfer public key to server from Windows client
- Enter the following command in powershell:
```bash
# point windows to public key file, open secure tunnel to user, change folder permissions, append public key to /.ssh/authorized_keys file
type $env:USERPROFILE\.ssh\id_ed25519.pub | ssh username@targetserverip "mkdir -p ~/.ssh && chmod 700 ~/.ssh && cat >> ~/.ssh/authorized_keys && chmod 600 ~/.ssh/authorized_keys"
```
> **For Linux clients**
> 
>  Replace previous command with ssh-copy-id
>  ```bash
>  ssh-copy-id username@targetserverip
>  ```

### Step 5: Connect to the server using the private key:
Step 5.1: Connect to remote server
```bash
# enter user and ip address you want to connect to
username@targetserverip
```
Step 5.2: Disable PasswordAuthentication
```bash
# use vi text editor to set the PasswordAuthentication parameter to no
sudo vi /etc/ssh/sshd_config
```

### Step 6: Verify that SSH key authentication works

## Definitions

## Commands

## Screenshots

## Expected results

## Verification

## Troubleshooting

## Security considerations

## References

## Revision history

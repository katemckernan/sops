# Set Up SSH Key Authentication on a Linux Server

## Purpose
This document acts as a guide for setting up SSH key authentication on a Linux server for remote administration. It follows security best practices, while maintaining consistency across our organizational standards.

## Audience
This document is intended for system administrators and IT involved in provisioning our organization's servers.

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
- Local admin credentials if SSH installation needed
- Target user credentials for remote access
- Root privileges on remote server
- Basic terminal navigation
- Network connection to the server
- Known IP address for local and remote server

## Technical Requirements
- **OpenSSH Client** on client system
- **OpenSSH Service** on remote Linux server
- **Supported Linux OS** on target server
- **IP configuration** on both systems
- **Firewall configuration** allowing inbound TCP connections on port 22

## Definitions

## Numbered procedure steps

## Commands

## Screenshots

## Expected results

## Verification

## Troubleshooting

## Security considerations

## References

## Revision history

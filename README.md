# Active Directory Homelab

A hands-on Active Directory infrastructure lab built with Windows
Server 2019, Windows 10, and Oracle VirtualBox.

The purpose of this project is to build and document a small
enterprise-style Windows domain environment, including Active Directory
Domain Services, DNS, DHCP, Organizational Units, users, security
groups, domain-joined clients, and Group Policy.

## Objectives

- Deploy an Active Directory Domain Controller
- Configure DNS and DHCP
- Design an Organizational Unit structure
- Create and manage domain users
- Create and manage security groups
- Join a Windows client to the domain
- Configure Group Policy Objects
- Validate GPO application on a domain client

## Topology Diagram

## IP Addressing Scheme

| Device   | IP Address   |      Role         |
|----------|--------------|-------------------|
| DC01     | 192.168.56.2 | Domain Controller |
| CLIENT01 | DHCP         | Domain Client     |

## Implementation

### VirtualBox Network Configuration

1. Host-Only Adapter Configuration
   
![View Image](network-configuration/virtualbox.png)

2. Windows 10 Network Configuration
   
![View Image](network-configuration/windows10-1.png)

![View Image](network-configuration/windows10-2.png)

3. Windows Server 2019 Network Configuration
   
![View Image](network-configuration/win-server-1.png)

![View Image](network-configuration/win-server-2.png)

### Windows 10 (CLIENT01) Network Configuration

![View Image](network-configuration/windows10-3.png)

![View Image](network-configuration/windows10-4.png)

### Windows Server 2019 (DC01) Network Configuration



### Active Directory Installation & Configuration

This section describes how to install and configure active directory in DC01

Server Manager -> Manage -> Add Roles and Features
Select Active Directory Services
Select Add Features
Select Install
Promote DC01 to Domain Controller
Select Add a New Forest, then set the Root domain name to `corp.local`
Set DSRM password
Select Install

### Setting up DHCP 

This section describes how to set up the DHCP server inside DC01, so that CLIENT01 can obtain its IP Address dynamically







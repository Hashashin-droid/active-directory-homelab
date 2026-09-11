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

![View Image](network-configuration/win-server-3.png)

![View Image](network-configuration/win-server-4.png)

### Active Directory Installation & Configuration

This section describes how to install and configure active directory in DC01

1. Server Manager -> Dashboard -> Add Roles and Features

![View Image](ad-configuration/ad-1.png)

2. Select Active Directory Services

![View Image](ad-configuration/ad-2.png)

3. Select Add Features

![View Image](ad-configuration/ad-3.png)

4. Select Install

![View Image](ad-configuration/ad-4.png)

5. Promote DC01 to Domain Controller

![View Image](ad-configuration/ad-5.png)

6. Select Add a New Forest, then set the Root domain name to `corp.local`

![View Image](ad-configuration/ad-6.png)

7. Set DSRM password

![View Image](ad-configuration/ad-7.png)

8. Select defaults in the following sections till you reach Prerequisites Check, then Select Install

![View Image](ad-configuration/ad-8.png)

### Setting up DHCP 

This section describes how to set up the DHCP server inside DC01, so that CLIENT01 can dynamically obtain its IP Address

1. Go Tools in Server Manager and select DHCP (DHCP services must be installed first)

![View Image](dhcp-configuration/dhcp-1.png)

2. Expand the server name, click IPv4, then choose New Scope

![View Image](dhcp-configuration/dhcp-2.png)

3. Click Next, then create a name for the Network Scope, and click Next

![View Image](dhcp-configuration/dhcp-3.png)

4. Create DHCP Pool : Configure Start Address, End Address, Subnet Mask

![View Image](dhcp-configuration/dhcp-4.png)

5. Add IP Exclusions (optional) : Add any specific IP Address that you want to reserve from the IP Address block

![View Image](dhcp-configuration/dhcp-5.png)

6. Set Lease Duration

![View Image](dhcp-configuration/dhcp-6.png)

7. Configure Options : Select Yes, I want to configure these options now

![View Image](dhcp-configuration/dhcp-7.png)

8. Configure Default Gateway (AD Static IP)

![View Image](dhcp-configuration/dhcp-8.png)

9. Activate Scope

![View Image](dhcp-configuration/dhcp-9.png)












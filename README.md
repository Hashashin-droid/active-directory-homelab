# Active Directory Homelab

Welcome to my Active Directory Homelab Project

This repository documents my hands-on experience building an Active Directory homelab using Windows Server 2019, Windows 10, and Oracle VirtualBox.

## Objectives

- Set up an Active Directory homelab environment
- Install and configure Active Directory Domain Services (AD DS)
- Set up DHCP
- Create Organizational Units (OUs)
- Create user accounts
- Create security groups
- Add users to security groups
- Move Windows clients into the appropriate OUs
- Create and apply Group Policy Objects (GPOs)
- Test and verify the GPO

## Topology Diagram

![Image](topology/topology-diagram.png)

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

This section describes how Active Directory Domain Services (AD DS) was installed and configured on DC01.

1. Server Manager -> Dashboard -> Add Roles and Features

![View Image](ad-configuration/ad-1.png)

2. Select Active Directory Domain Services

![View Image](ad-configuration/ad-2.png)

3. Select Add Features

![View Image](ad-configuration/ad-3.png)

4. Select Install

![View Image](ad-configuration/ad-4.png)

5. Promote DC01 to Domain Controller

![View Image](ad-configuration/ad-5.png)

6. Select Add a New Forest, then set the Root domain name to `corp.local`

![View Image](ad-configuration/ad-6.png)

7. Set Directory Services Restore Mode (DSRM) password

![View Image](ad-configuration/ad-7.png)

8. Continue through the remaining configuration sections using the appropriate settings. At Prerequisites Check, select Install

![View Image](ad-configuration/ad-8.png)

### Setting up DHCP 

This section describes how DHCP was configured on DC01 so that CLIENT01 can dynamically obtain an IP address.

1. Open Server Manager → Tools → DHCP. The DHCP Server role must be installed first

![View Image](dhcp-configuration/dhcp-1.png)

2. Expand the server name, click IPv4, then choose New Scope

![View Image](dhcp-configuration/dhcp-2.png)

3. Select Next, enter a name for the network scope, and continue

![View Image](dhcp-configuration/dhcp-3.png)

4. Create the DHCP address pool by configuring the start address, end address, and subnet mask.

![View Image](dhcp-configuration/dhcp-4.png)

5. Add IP Exclusions (optional) : Add any specific IP Address that you want to reserve from the IP Address block

![View Image](dhcp-configuration/dhcp-5.png)

6. Configure the lease duration

![View Image](dhcp-configuration/dhcp-6.png)

7. Configure DHCP options by selecting Yes, I want to configure these options now

![View Image](dhcp-configuration/dhcp-7.png)

8. Configure Default Gateway (AD Static IP)

![View Image](dhcp-configuration/dhcp-8.png)

9. Activate the DHCP scope

![View Image](dhcp-configuration/dhcp-9.png)

### Setting Up Organizational Units (OUs)

This section describes on how to create Organizational Units (OUs)

Organizational Units is a logical structure inside an AD domain that holds network objects like users, computers, and groups to make management and security easier

- For this lab, I created a Department OU with two child OUs: IT Support and Finance

<img width="1022" height="788" alt="650771649-94e949c8-546c-4a73-9c89-3d1572f43de2" src="https://github.com/user-attachments/assets/e9ecddeb-e896-4f59-bee4-c15d3bd04ddf" />

- It is good practice to enable Protect container from accidental deletion for important OUs. This helps prevent an OU from being deleted unintentionally

<img width="1022" height="788" alt="650771799-b4427f70-01c3-4cc4-8cbf-58975b897527" src="https://github.com/user-attachments/assets/aa3a0b43-97db-4276-b611-5d029be7aa38" />

- Anyway, if an OU has accidental-deletion protection enabled, the protection must be disabled before the OU can be deleted

<img width="1022" height="788" alt="650771799-b4427f70-01c3-4cc4-8cbf-58975b897527" src="https://github.com/user-attachments/assets/fb0b5951-e2c6-484a-8d10-c6b75d2fcd9c" />

### Creating Users

A user account is an object that holds all the information or attributes that define a user. With user account, a user can provide authentication to AD DS domain and access network resources

For this lab, I created user accounts in the IT Support and Finance OUs. The configured attributes include:
- First name
- Last name
- User logon name
- Password 

<img width="1022" height="788" alt="650776173-51157758-5348-415d-b580-0fbec627bf39" src="https://github.com/user-attachments/assets/484d3be4-8208-4cb1-a577-0ffd9747f4c5" />


### Creating Security Groups

In AD, a group is a collection of users, computers, or other group accounts that can be managed together. So, instead of assigning permissions to user individually, we can place users into a group and assign permissions to the group

#### Types of Groups
- **Security** -  Use these to assign permissions to shared network resources like folders, printers, and files. You can also use them to apply Group Policy settings
- **Distribution** - Use these only for email distribution lists in Microsoft Exchange or Outlook. You cannot use them to assign Windows security permissions

#### Group Scope
Group scope defines where a group's permissions apply and what members the group can contain:
- **Domain local** - Use these to assign permission to resources within their domain
- **Global** -  Use these to organize users or computers who share the same job roles or department tasks inside a single domain
- **Universal** -  Use these in large, multi-domain forests to combine groups across different domains

For this lab, I created two additional Global Security Groups based on departmental roles:
- `Supervisors` in Finance
- `IT Support` in Helpdesk

Both groups were configured with Global scope and Security type, Security was selected because these groups are intended for access control, while Global scope is suitable for grouping users based on roles within the same AD domain.

<img width="1022" height="788" alt="650777608-eb375a39-f80f-4cb5-9ea1-653d38ff2296" src="https://github.com/user-attachments/assets/45409f1c-2e50-405f-920f-5f3b3a7517ee" />


### Adding Users to Groups

Next, I added the previously created user accounts to their corresponding security groups

<img width="1022" height="788" alt="650777925-237c441c-fa6e-430c-a05f-5aeb4049821d" src="https://github.com/user-attachments/assets/9ad5c852-38e7-4eac-9710-e9f1ef82fb91" />

### Moving CLIENT01 to the respective OU

I moved `CLIENT01` into `Finance` so that it can be used to demonstrate the implementation of Group Policy Objects (GPOs) later in the project

<img width="1022" height="788" alt="650779081-5b0fc0f1-120b-4e95-b8b0-097823cbd77a" src="https://github.com/user-attachments/assets/cf94fd98-3067-4494-b54f-a5db7be90ddf" />

### Create and Push Group Policy Object (GPO)

Group Policy is one of the core features that make AD a powerful tool for enterprise IT management. With it, IT administrators can define rules and configuration settings that apply uniformly across users and devices (from password policies to desktop configurations) without having to configure each machine individually

For demonstration purposes, I created a GPO that displays a message and notification when users log in. This provides a simple way to verify that the GPO is applied successfully.

<img width="1022" height="789" alt="650781268-45aaa16b-3635-451b-ab38-321a0ad144ab" src="https://github.com/user-attachments/assets/418bd2d2-9100-4677-a49d-4f3a7faf26c6" />

### Test the GPO

The GPO has been successfully applied to the virtual machine `CLIENT01`, as demonstrated below.

[650782083-c0413e27-1886-46ea-9137-6103b733bd65.webm](https://github.com/user-attachments/assets/fdfd6a15-8bb6-4917-b1d6-ee1f748e5469)





























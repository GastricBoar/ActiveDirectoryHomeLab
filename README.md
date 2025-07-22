# Porto.lab ⚓

Active Directory lab simulating the IT backbone of a modern commercial port (inspired by port of Rotterdam) and a logistics center (in Utrecht).\
I ignore how a commercial port really operates, i just like ports.

## Summary
- [How this works](#how-this-works)
- [Powershell script to create new users](#powershell-script-to-create-new-users)
- [Department structure](#department-structure)
- [DNS role](#dns-role)
- [DHCP role](#dhcp-role-dynamic-allocations)
- [Using Docker and MacVLAN driver to fake network devices](#using-docker-and-macvlan-driver-to-fake-network-devices)
- [GPOs](#gpos)
- [File server (access based enumeration, quota management, file screening)](#file-server-access-based-enumeration-quota-management-file-screening)
- [Service accounts](#service-accounts)
- [Active Directory control delegations](#active-directory-control-delegations)
- [Bookstack wiki (with LDAP authentication)](#bookstack-wiki-with-ldap-authentication)
- [Next steps](#next-steps)


## How this works
I used VMware Workstation to create two machines:

- **Server**: Windows Server 2022 as  
  - Domain Controller (AD – porto.lab)  
  - DNS  
  - DHCP (172.16.0.100–200)

- **Client**: Windows 10 joined to the domain

- **Application server**: Ubuntu Server hosting containers and services

<img width="821" height="521" alt="Active Directory Home Lab Diagram" src="https://github.com/user-attachments/assets/c9d209f5-4f08-4806-91ce-f92f2ce78f0a" />



## PowerShell script to create new users
I created a text file, each line includes a name, a office location and a department; i then setup a Powershell script that creates accounts in Active Directory, it bulk-imports names from the text file, sets office location and department attributes for each one.

## Department structure
Each user is assigned to one of these offices: Environment & Safety, Administration, Concession Companies, Management, Customs, Port Operations, Human Resources, IT Systems, Technical Office, Security.

My organizational units reflect administrative task needs, they are not necessarily a 1:1 copy of my organization chart.

There's a couple security groups, and distribution lists that manage internal communications.
 
![immagine](https://github.com/user-attachments/assets/12f1cd99-5fd0-4729-8a5d-9eaa5bc0c869)



## DNS role
Server is the primary DNS for the lab. All domain-joined clients query the internal DNS for resolution, if that doesn't succeed the query gets forwarded to the upstream public DNS.

![immagine](https://github.com/user-attachments/assets/a944c92c-eebf-4f0e-b36c-b586f02fe687)

## DHCP role (dynamic allocations)
Server issues dynamic IP addresses in the 172.16.0.100 – 172.16.0.200 range to all clients.

![immagine](https://github.com/user-attachments/assets/4d8b8b83-f2a8-43a7-8544-14dd433ab6f3)

## Using Docker and MacVLAN driver to fake network devices
I wanted to validate DHCP reservations and simulate realistic network interactions, but it's not like i have printers or IP phones at home. Instead, i deployed Docker containers using MacVLAN network driver. This allows containers to appear on the LAN with their own MAC and IP addresses. Containers are based on small Ubuntu images that i customized to include a DHCP client and network tools.

<img width="1139" height="144" alt="immagine" src="https://github.com/user-attachments/assets/fa2e5456-267e-4cab-85d2-cea91021f191" /><br>

This worked out well in the end! IP addresses in the 172.16.0.02 – 172.16.0.99 range will be used for reservations.<br>

<img width="239" height="133" alt="immagine" src="https://github.com/user-attachments/assets/0a5fdbb2-b392-42e6-8d57-5918af0cc541" />


## GPOs
I have a couple test GPOs up in place: drive maps, set wallpapers, restrict panel control access, block access to USB devices, password and lockout policy, deny server log on to non-admin users, allow remote server access to admins, deny non-admin users access to service accounts.

![immagine](https://github.com/user-attachments/assets/af87a6c8-9232-4a62-96bf-16441ae61efb)

## File server (access based enumeration, quota management, file screening)
I setup a file server with shared folders for every department, used NTFS permissions and Access Based Enumeration to make sure everybody sees only what they need to see according to their role.

There's quota management and file screening (blocking audio/video) to help tidy things up.

![immagine](https://github.com/user-attachments/assets/fd752119-ac90-412c-8912-481ce41d3263)

## Service accounts
I use a service account for a kiosk screen right at the entrance of my port. I configured it to autologin, automatically start a browser on a specific web page in fullscreen mode. Non-admin users cannot access service accounts, this is enforced through GPO.

<img src="https://github.com/user-attachments/assets/07239b12-edda-428c-8b6e-97edfb7ab654" alt="Kiosk for my port" width="700">

## Active Directory control delegations
Imagined scenario is "helpdesk operators can access the domain controller server, they can read AD but are only allowed to reset user passwords". I delegated AD control to achieve this, making sure they could only act on a specific set of users.

![immagine](https://github.com/user-attachments/assets/9b74027d-5c94-4da4-b8e5-aa4150b61c05)

## Bookstack wiki (with LDAP authentication)
I setup a Bookstack installation on my Ubuntu server. Users access it through the "wiki.lab" domain name, and they authenticate through LDAP. Only users in the "Bookstack users" AD group can access the wiki.

<img width="1849" height="493" alt="immagine" src="https://github.com/user-attachments/assets/58ffc5ae-0278-4804-8b01-ed79531be835" />


## Next steps
Linux server domain join, LLM-based chatbot for knowledge, network devices simulation using Docker MacVLAN, powershell script repository for common AD tasks, Veeam backups, monitoring software.



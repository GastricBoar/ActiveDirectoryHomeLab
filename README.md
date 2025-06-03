# Porto.lab ⚓

Active Directory lab simulating the IT backbone of a modern commercial port (inspired by port of Rotterdam) and a logistics center (in Utrecht).\
I ignore how a commercial port really operates, i just like ports.


## How this works
I used VMware Workstation to create two machines:

- **Server**: Windows Server 2022 as  
  - Domain Controller (AD – porto.lab)  
  - DNS  
  - DHCP (172.16.0.100–200)

- **Client**: Windows 10 joined to the domain

![Active Directory Home Lab(1)](https://github.com/user-attachments/assets/1f00efc1-0a42-4cb1-bda0-74ba8936004c)


## PowerShell script to create new users
I created a text file, each line includes a name, a office location and a department; i then setup a Powershell script that creates accounts in Active Directory, it bulk-imports names from the text file, sets office location and department attributes for each one.

## Department structure
Each user is assigned to one of these offices: Environment & Safety, Administration, Concession Companies, Management, Customs, Port Operations, Human Resources, IT Systems, Technical Office, Security.

There's a couple security groups, and distribution lists that manage internal communications.
 
![immagine](https://github.com/user-attachments/assets/12f1cd99-5fd0-4729-8a5d-9eaa5bc0c869)



## DNS role
Server is the primary DNS for the lab. All domain-joined clients query the internal DNS for resolution, if that doesn't succeed the query gets forwarded to the upstream public DNS.

![immagine](https://github.com/user-attachments/assets/a944c92c-eebf-4f0e-b36c-b586f02fe687)

## DHCP role
Server issues IP addresses in the 172.16.0.100 – 172.16.0.200 range to all clients.

![immagine](https://github.com/user-attachments/assets/4d8b8b83-f2a8-43a7-8544-14dd433ab6f3)

## GPOs
I have a couple test GPOs up in place: drive maps, set wallpapers, restrict panel control access, block access to USB devices, a password and lockout policy.

![immagine](https://github.com/user-attachments/assets/c3ad59be-c9bf-46dc-b7f7-20be17bfeccb)


## Next steps
- **File server**: shared folders, NTFS permissions, quota management  
- **Security & Automation**: audit policies, LAPS, PowerShell DSC/Ansible  
- **Hybrid experiments**: Azure AD Connect, Terraform-driven IaC  



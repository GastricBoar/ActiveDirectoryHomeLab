# Porto.lab ⚓

Active Directory lab simulating the IT backbone of a modern commercial port (inspired by port of Rotterdam).\
I ignore how a commercial port really operates, i just like ports.


## How this works
I used VirtualBox to create two machines:

- **Server**: Windows Server 2022 as  
  - Domain Controller (AD – porto.lab)  
  - DNS  
  - DHCP (172.16.0.100–200)/

- **Client**: Windows 10 joined to the domain

![Active Directory Home Lab - network diagram](https://github.com/user-attachments/assets/df941814-9ce2-4c39-b1c9-f81e1fa2530a)

## What's in place
- **AD Structure**  
  - Users, organized by office (Environment & Safety, Administration, Concession Companies, Management, Customs, Port Operations, Human Resources, IT Systems, Technical Office, Security)
  - PowerShell scripts to bulk-import users and set the office attribute
 
![immagine](https://github.com/user-attachments/assets/18f643a3-390d-497c-bcbd-312d13943e23)


- **Network Services**  
  - Internal DNS for porto.lab  
  - DHCP scope and options  

## Next steps

- **GPO**: drive mappings, USB restrictions, custom lock-down policies  
- **File server**: shared folders, NTFS permissions, quota management  
- **Security & Automation**: audit policies, LAPS, PowerShell DSC/Ansible  
- **Hybrid experiments**: Azure AD Connect, Terraform-driven IaC  



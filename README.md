## Windows Server DNS and DHCP Lab

### Overview
- This lab demonstrates Windows Server DNS and DHCP configuration in an Active Directory domain environment.
- The goal of this lab was to configure DNS records, create a DHCP scope, assign IP addresses automatically to a Windows client, configure a DHCP reservation, and verify hostname resolution using Command Prompt tools.

----------------------------------------------------
### Lab Environment
- Server Name: DC1
- Server OS: Windows Server 2022
- Client Name: PC1
- Client OS: Windows 11
- Domain: amir.local
- Network: VirtualBox Internal Network - AD-Lab  

----------------------------------------------------
### IP Addressing
- DC1 Static IP: 192.168.10.10
- DHCP Scope Range: 192.168.10.100 - 192.168.10.200
- Reserved IP for PC1: 192.168.10.50
- Subnet Mask: 255.255.255.0
- DNS Server: 192.168.10.10  

----------------------------------------------------
### DNS Configuration
- Forward Lookup Zone: amir.local
- Reverse Lookup Zone: 192.168.10.0/24  

DNS A Record:
- fileserver.amir.local → 192.168.10.20  

DNS CNAME Record:
- files.amir.local → fileserver.amir.local  

----------------------------------------------------
### DHCP Configuration
- Scope Name: AD-Lab-Scope
- Scope Range: 192.168.10.100 - 192.168.10.200
- Exclusion Range: 192.168.10.100 - 192.168.10.109
- DNS Server Option: 192.168.10.10
- DNS Domain Option: amir.local
- Reservation: PC1 → 192.168.10.50
- PC1 was configured with a DHCP reservation to always receive 192.168.10.50, while the general DHCP pool assigns addresses from 192.168.10.100 to 192.168.10.200.

----------------------------------------------------
### Skills Demonstrated
- Windows DNS management
- Forward lookup zone verification
- Reverse lookup zone creation
- DNS A record creation
- DNS CNAME record creation
- Windows DHCP installation
- DHCP authorization in Active Directory
- DHCP scope creation
- DHCP scope options
- DHCP lease verification
- DHCP reservation configuration
- Client IP renewal
- Hostname resolution troubleshooting  

----------------------------------------------------
### Verification Commands
- ipconfig /all
- ipconfig /release
- ipconfig /renew
- ping dc1
- nslookup amir.local
- nslookup dc1.amir.local
- nslookup fileserver.amir.local
- nslookup files.amir.local  

----------------------------------------------------
### Troubleshooting Notes

### Issue:
- PC1 does not receive a DHCP address.

Cause:
- The DHCP scope may not be active, the DHCP server may not be authorized, PC1 may not be set to automatic IP, or both VMs may not be on the same internal network.

Fix:
- Authorize DHCP, activate the scope, set PC1 to obtain an IP automatically, confirm both VMs use the AD-Lab internal network, and run ipconfig /renew.

### Issue:
- PC1 receives an IP address but DNS does not work.

Cause:
- DHCP option 006 may be missing or incorrect.

Fix:
- Set DHCP option 006 to 192.168.10.10 and renew the client DHCP lease.

### Issue:
- DNS record does not resolve.

Cause:
- The A or CNAME record may be missing, created in the wrong zone, or PC1 may have old DNS cache.

Fix:
- Verify the record in DNS Manager and run ipconfig /flushdns on PC1.

----------------------------------------------------
### Final Result
- Windows DHCP successfully assigned an IP address to PC1. DHCP scope options provided the correct DNS server and domain name. A DHCP reservation was configured for PC1. DNS A and CNAME records were created and tested successfully using nslookup.
- This lab demonstrates practical Windows Server DNS, DHCP, client IP assignment, hostname resolution, and troubleshooting skills.

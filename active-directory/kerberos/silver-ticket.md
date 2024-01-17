# Silver Ticket

A silver ticket is a type of attack where an attacker forge the TGT of a service account. This means that an attacker with a Silver Ticket can impersonate a specific user to access a particular service. By default, computer passwords change every 30 days, at which time you must re-obtain the new secrets to continue making silver tickets.

### **Attack Prerequisites**

* User's SID
* User's hashed password _**OR**_ access to krbtgt password hash

### **Purpose of Silver Ticket**

The silver ticket attack is a post exploitation attack where an attacker has already compromised a domain user account. It is primarily **used for host persistence** instead of privilege escalation.

With a silver ticket, attacker can persistently impersonate any user to any service on that machine.

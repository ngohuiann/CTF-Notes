# Access Control
[HOME](/index.html) / [Web](/Web)

## The basics
Attacker gaining access to unauthorized content.

## Access aggregation 
Access Aggregation is the collection of nonsensitive data and processing it to learn sensitive information. \
For example:\
&nbsp;&nbsp;&nbsp;&nbsp;Attacker may try to guess the target's location by retrieve their IP address.\
&nbsp;&nbsp;&nbsp;&nbsp;Implement features to prevent access aggregation attack:
<ul><li>Defense-in-depth</li>
<li>Role-based access control</li>
<li>Least privilege principle</li></ul>


## Password attacks
Password is a form of authentication method that can be easily exploited by malicious user when not properly coded.<br /><br />
There are several types of password attacks:
<ul><li>Dictionary Attack</li>
<li>Rainbow Table Attack</li>
<li>Credential Stuffing</li></ul>

### Dictionary Attack
Dictionary attack is a type of brute forcing attack to guess a password. It runs through a list of commonly used password (eg. 12345678, abc12345...) until it finds a matching one.

### Rainbow Table Attack
Usually password stored in a database are hashed, a rainbow table is a database that is able to crack the hash value using precomputed dictionary of plaintext password.

### Credential Stuffing
A credential stuffing attack involved the use of stolen credential from other services to break into accounts on other services. This is an effective and commonly seen attack due to the large amount of username and password reuse in different system.


## Spoofing attacks
Spoofing is the act of pretending to be something.\
Type of spoofing:
<ul><li><b>Web Spoofing</b></li>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Web spoofing is commonly done with the combination of phishing attack. The attacker develop a website that looks the same as an existing site and send it to the victim to retrieve their login credential.
<li><b>IP Spoofing</b></li>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;IP spoofing attack happen when the attacker replace their IP address with a false one to impersonate someone or hide their identity to gain access into a system. 
<li><b>ARP Spoofing</b></li>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;ARP spoofing is done to link the attacker's MAC address to the victim's IP address. Any traffic sent to the victim will also be received by the attacker.</ul>


## Social engineering attacks
Social Engineering is a type of exploitation technique that look into manipulating human to give confidential information.\
Example of Social Engineering Attack:
<ul><li>Receiving email from the attacker masked to be a friend asking for password.</li>
<li>Receive calls claiming from banks asking for credit card information.</li>
<li>Posing as colleage or customers asking for confidential data.</li></ul>

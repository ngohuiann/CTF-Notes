# Active Directory Environment Set Up

## Common Terminology <a href="#bkmrk-common-terminology" id="bkmrk-common-terminology"></a>

Get familiar with these terms before you moved on:

| Abbreviation     | Full Form                             |
| ---------------- | ------------------------------------- |
| AD               | Active Directory                      |
| CA               | Certificate Authority                 |
| CS               | Certificate Services                  |
| DC               | Domain Controller                     |
| DCOM             | Distributed Component Object Model    |
| DPAPI            | Data Protection API                   |
| LDAP             | Lightweight Directory Access Protocol |
| <p>MS<br></p>    | <p>Member Server<br></p>              |
| PKI              | Public Key Infrastructure             |
| SAM              | Service Account Management            |
| SPN              | Service Principal Name                |
| TGS              | Ticket Granting Service               |
| TGT              | Ticket Granting Ticket                |
| <p>WinRM<br></p> | <p>Windows Remote Management<br></p>  |
| WMI              | Windows Management Instrumentation    |

## Domain Controller & Member Server <a href="#bkmrk-domain-controller-26" id="bkmrk-domain-controller-26"></a>

In an AD environment, a DC is the main server that controls all MS.

* **DC** can be any PC, workstation, laptop or server running **Windows Server OS** (or Linux OS with AD service installed).
* **MS** can be any PC, workstation, laptop, IoT devices, servers, databases (and more) running **normal OS** (yes, even Linux OS, just more complicated to configure).

Windows OS Home Edition does not support joining domain.

### **Setting Up DC**

Install a Windows Server OS as you normally would with a normal Windows OS. Windows Server 2019 Essentials ISO can be downloaded [here (scroll to bottom of page for license key)](https://www.microsoft.com/en-us/evalcenter/download-windows-server-2019-essentials) and [Windows 11](https://www.microsoft.com/software-download/windows11) if you want to follow along this guide.\


Before we start anything, the default password for the built in Administrator account is empty. You have to set a strong password that meets the complexity requirement. To set the Administrator user password, go to Windows Run (win + R) and type _lusrmgr.msc._ Right click on the Administrator account and set password.

<details>

<summary>Configurations to be made beforehand if you want to follow along</summary>

To create a AD environment, all PC must be within the same network. Therefore, to ease the process, we will be using Host-Only network adapter on our VM.

This is the configuration I have for this walkthrough, you may modify it as you wish.\


To set up a host-only adapter in VMware Workstation, launch Virtual Network Editor with Administrator right.\


[![image.png](http://192.168.1.119/uploads/images/gallery/2024-01/scaled-1680-/cB3anCAFyc8Bwxyd-image.png)](http://192.168.1.119/uploads/images/gallery/2024-01/cB3anCAFyc8Bwxyd-image.png)

&#x20;You may choose to modify on existing Host-only network adapter or Add Network.

[![image.png](http://192.168.1.119/uploads/images/gallery/2024-01/scaled-1680-/HaRQDYXR2FbqFkvl-image.png)](http://192.168.1.119/uploads/images/gallery/2024-01/HaRQDYXR2FbqFkvl-image.png)

&#x20;Select your existing network number or newly create network, then choose Host-Only.\


[![image.png](http://192.168.1.119/uploads/images/gallery/2024-01/scaled-1680-/xB1jyxkHY7TPleTB-image.png)](http://192.168.1.119/uploads/images/gallery/2024-01/xB1jyxkHY7TPleTB-image.png)

Then, go to DHCP Settings and make your desired changes. Just ensure the Stating/Ending IP address is in the same subnet within the subnet IP.\


[![image.png](http://192.168.1.119/uploads/images/gallery/2024-01/scaled-1680-/xZQI4cGeBUa1kel9-image.png)](http://192.168.1.119/uploads/images/gallery/2024-01/xZQI4cGeBUa1kel9-image.png)

</details>

Windows Server has a special built in application that is not present in the normal Windows OS. That is the Server Manager:

[![image.png](http://192.168.1.119/uploads/images/gallery/2024-01/scaled-1680-/AaetUpyuP9b3diRn-image.png)](http://192.168.1.119/uploads/images/gallery/2024-01/AaetUpyuP9b3diRn-image.png)

1\. To create an AD domain, go to Manage -> Add Roles and Features.

[![image.png](http://192.168.1.119/uploads/images/gallery/2024-01/scaled-1680-/SItX9jE6IiwWCW25-image.png)](http://192.168.1.119/uploads/images/gallery/2024-01/SItX9jE6IiwWCW25-image.png)

2\. Leaving all options by default, click "Next" until you reach the "Server Roles" tab. Check the "Active Directory Domain Services" checkbox.

[![image.png](http://192.168.1.119/uploads/images/gallery/2024-01/scaled-1680-/IE88jhwBX47BS5NB-image.png)](http://192.168.1.119/uploads/images/gallery/2024-01/IE88jhwBX47BS5NB-image.png)

3\. Continue to click on Next until you reach the Confirmation tab and select Install.

[![image.png](http://192.168.1.119/uploads/images/gallery/2024-01/scaled-1680-/2Fr4XepHlFu84dLL-image.png)](http://192.168.1.119/uploads/images/gallery/2024-01/2Fr4XepHlFu84dLL-image.png)

4\. Let the installer run, and we will continue after the installation completes.

[![image.png](http://192.168.1.119/uploads/images/gallery/2024-01/scaled-1680-/3MRuDYRhBFxGBnWE-image.png)](http://192.168.1.119/uploads/images/gallery/2024-01/3MRuDYRhBFxGBnWE-image.png)

5\. When the installation is completed, click on the "Promote this server to a domain controller".\


[![image.png](http://192.168.1.119/uploads/images/gallery/2024-01/scaled-1680-/z3UQP4ml3zXCKJJW-image.png)](http://192.168.1.119/uploads/images/gallery/2024-01/z3UQP4ml3zXCKJJW-image.png)

6\. This will open up another window. Select option "Add a new forest" and enter your desired domain name.

We will be using **santaa.net** as the domain name throughout this guide. You may change it to anything you like.

[![image.png](http://192.168.1.119/uploads/images/gallery/2024-01/scaled-1680-/kz6nKgwQG8zrxFaZ-image.png)](http://192.168.1.119/uploads/images/gallery/2024-01/kz6nKgwQG8zrxFaZ-image.png)

7\. Enter your desired DSRM password for this AD.

[![image.png](http://192.168.1.119/uploads/images/gallery/2024-01/scaled-1680-/QJQEo6Ijt4PkyTE7-image.png)](http://192.168.1.119/uploads/images/gallery/2024-01/QJQEo6Ijt4PkyTE7-image.png)

8\. Since this is just a simple walkthrough of how you can create a domain, you can leave everything else by the default option and click on Next until you reach the Prerequisites Check tab. Click on Install and wait for the installation to complete. The server will automatically restart during installation, so don't freak out when it does.\


If your prerequisites check is unsuccessful, read the error message and see where went wrong.

[![image.png](http://192.168.1.119/uploads/images/gallery/2024-01/scaled-1680-/sWslcLUqV4fQKKoC-image.png)](http://192.168.1.119/uploads/images/gallery/2024-01/sWslcLUqV4fQKKoC-image.png)

9\. The domain set up is completed once the OS is done restarting. You can check on the Server Manager -> Local Server to confirm. You can also change the Computer Name to something more recognizable.

[![image.png](http://192.168.1.119/uploads/images/gallery/2024-01/scaled-1680-/s7qXQUk6muRIGyGN-image.png)](http://192.168.1.119/uploads/images/gallery/2024-01/s7qXQUk6muRIGyGN-image.png)



***

### **Joining Domain**

To join a PC to a domain, the PC will need to be on the same network as the DC.

1\. Do "ipconfig" on the DC to get the server's IP.

[![image.png](http://192.168.1.119/uploads/images/gallery/2024-01/scaled-1680-/Q9gu5VhzURjnog3U-image.png)](http://192.168.1.119/uploads/images/gallery/2024-01/Q9gu5VhzURjnog3U-image.png)

2\. On your PC that you wanted to join domain, set the DNS server address of the Network Adapter to your DC's IP address.

In real life enterprise environment, configuring DNS server will be more complicated. This is only a simplified process for the sake of learning.

[![image.png](http://192.168.1.119/uploads/images/gallery/2024-01/scaled-1680-/QyFRiZw38YI9BRn3-image.png)](http://192.168.1.119/uploads/images/gallery/2024-01/QyFRiZw38YI9BRn3-image.png)

To make sure everything works, you can try to ping the domain.

[![image.png](http://192.168.1.119/uploads/images/gallery/2024-01/scaled-1680-/0D3AZWXUB1SQbWhp-image.png)](http://192.168.1.119/uploads/images/gallery/2024-01/0D3AZWXUB1SQbWhp-image.png)

[![image.png](http://192.168.1.119/uploads/images/gallery/2024-01/scaled-1680-/2IpZzdfPOrEk4JmZ-image.png)](http://192.168.1.119/uploads/images/gallery/2024-01/2IpZzdfPOrEk4JmZ-image.png)

3\. Go to System Properties -> Change... -> Member of Domain

[![image.png](http://192.168.1.119/uploads/images/gallery/2024-01/scaled-1680-/mhTGClgSuPKqJgMh-image.png)](http://192.168.1.119/uploads/images/gallery/2024-01/mhTGClgSuPKqJgMh-image.png)

[![image.png](http://192.168.1.119/uploads/images/gallery/2024-01/scaled-1680-/Quy8TszXeM2av1Js-image.png)](http://192.168.1.119/uploads/images/gallery/2024-01/Quy8TszXeM2av1Js-image.png)

4\. Enter the Domain Admin's username and password when prompt.

[![image.png](http://192.168.1.119/uploads/images/gallery/2024-01/scaled-1680-/gV1AIprloUzUv4yE-image.png)](http://192.168.1.119/uploads/images/gallery/2024-01/gV1AIprloUzUv4yE-image.png)

[![image.png](http://192.168.1.119/uploads/images/gallery/2024-01/scaled-1680-/WZy3048IQhvA26OT-image.png)](http://192.168.1.119/uploads/images/gallery/2024-01/WZy3048IQhvA26OT-image.png)

Restart your PC for the change to take affect.

Devices joined to domain are referred as Member Server (MS).\


<details>

<summary>Very basic things that's easily missed but you should probably know</summary>

**How to know if a PC is joined to domain?**

1\. On a device that is already logged in, its pretty straight forward, just refer to the System Properties like mentioned above in the Joining Domain section.

2\. On a device that is not logged in yet:

After a device is joined to domain, by default the Windows sign in method is changed to sign in using domain account instead of local account.

When signing in to devices joined to domain using domain account, the device must be able to reach to the DC. That means that the DC and MS must be within the same network/reachable through internet (Cloud/however it is configured).

If you want to sign into a local account, you have to append **.\\** at the start of your local account username.

Example when signing into a **domain account**, the domain is specified in the sign in menu:

[![image.png](http://192.168.1.119/uploads/images/gallery/2024-01/scaled-1680-/XD855qyAprxJJ6Hz-image.png)](http://192.168.1.119/uploads/images/gallery/2024-01/XD855qyAprxJJ6Hz-image.png)

If you want to sign into another domain, you can specify username in the form of \[domain]\\\[username] _or_ \[username]@\[domain].

[![image.png](http://192.168.1.119/uploads/images/gallery/2024-01/scaled-1680-/QC7NHkx0eB3YO7ws-image.png)](http://192.168.1.119/uploads/images/gallery/2024-01/QC7NHkx0eB3YO7ws-image.png)

Notice how the name changes when you specify a domain.

\


&#x20;

***

&#x20;

To sign into a **local account**:

Username should be in the form of .\\\[username]. The name changes back to the PC hostname, this indicate that we are trying to sign in as a local user.\


[![image.png](http://192.168.1.119/uploads/images/gallery/2024-01/scaled-1680-/5ahHETlTKOFgAx1R-image.png)](http://192.168.1.119/uploads/images/gallery/2024-01/5ahHETlTKOFgAx1R-image.png)

**Domain username and password**

In a real world enterprise scenario, your domain account is usually connected with your enterprise email. For example, if my email is [hui.ann@santaa.com](mailto:huiann@santaa.com), my domain account username would be hui.ann.\


Changing the password for my email [hui.ann@santaa.com](mailto:huiann@santaa.com) will also change the password I would need to use to login to my PC that is joined to domain and vice versa.

</details>

## **Recognizing a DC**

A DC has certain ports that are always open so that other MS can communicate with them. When you do Nmap scan on a IP and see these ports open, there is a high probability (not 100%) its a DC:

<table data-full-width="false"><thead><tr><th width="283">Default Port</th><th>Name</th></tr></thead><tbody><tr><td>22</td><td>SSH</td></tr><tr><td>53</td><td>DNS</td></tr><tr><td>88</td><td>Kerberos</td></tr><tr><td>135</td><td>RPC</td></tr><tr><td>139</td><td>NetBIOS</td></tr><tr><td>389</td><td>LDAP</td></tr><tr><td>445</td><td>SMB</td></tr><tr><td>636<br></td><td>LDAPS</td></tr><tr><td>3389</td><td>RDP</td></tr></tbody></table>

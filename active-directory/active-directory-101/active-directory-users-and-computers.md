# Active Directory Users and Computers

In DC server, there is a built in tool that allow easy management to all objects in the domain. The Active Directory Users and Computers is an application that comes along when we install Active Directory Domain Services.

In case you are wondering, it can be found in the start menu -> Windows Administrative Tools.

Expand your domain in the left menu to see more options.

[![image.png](http://192.168.1.119/uploads/images/gallery/2024-01/scaled-1680-/09lDNEWIVYauClmd-image.png)](http://192.168.1.119/uploads/images/gallery/2024-01/09lDNEWIVYauClmd-image.png)

## **Computers**

In our previous article, we discussed about joining a PC to this domain. This PC can now be been under the Computers object.

[![image.png](http://192.168.1.119/uploads/images/gallery/2024-01/scaled-1680-/bWh6qnl4VZleiQAo-image.png)](http://192.168.1.119/uploads/images/gallery/2024-01/bWh6qnl4VZleiQAo-image.png)

If you right click on the PC name, you can make various changes to the PC remotely. For now, lets go into **Properties**.

[![image.png](http://192.168.1.119/uploads/images/gallery/2024-01/scaled-1680-/xY4vRUvJSSsp6SoC-image.png)](http://192.168.1.119/uploads/images/gallery/2024-01/xY4vRUvJSSsp6SoC-image.png)

This window will show detailed information about the computer, you may explore around on your own.&#x20;

Let move on to the "**Member Of**" tab. This tab will show what member is this computer under.

[![image.png](http://192.168.1.119/uploads/images/gallery/2024-01/scaled-1680-/NUU0oFX4gkSFGjGC-image.png)](http://192.168.1.119/uploads/images/gallery/2024-01/NUU0oFX4gkSFGjGC-image.png)

***

## **Users**

If you are using a domain admin account, you will have the privilege to add, modify, delete an user account.

[![image.png](http://192.168.1.119/uploads/images/gallery/2024-01/scaled-1680-/c6F5FzGoFDqdG7uG-image.png)](http://192.168.1.119/uploads/images/gallery/2024-01/c6F5FzGoFDqdG7uG-image.png)

When doing Network Assessment with Nessus credential check, sometimes account will get locked due to the limit of password attempt. As a domain admin, you can unlock the user account by right clicking on the user and select **Reset Password**.

[![image.png](http://192.168.1.119/uploads/images/gallery/2024-01/scaled-1680-/1AAI16yjPnlxlsho-image.png)](http://192.168.1.119/uploads/images/gallery/2024-01/1AAI16yjPnlxlsho-image.png)

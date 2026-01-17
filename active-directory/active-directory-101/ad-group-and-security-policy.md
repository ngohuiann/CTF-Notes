# AD Group and Security Policy

Active Directory (AD) groups are like collections or clubs that gather users, computers, or other groups together for easier management and organization within a company's network.<br>

Many organization or enterprise will create separate groups for each department.

## **Creating AD Group**

Open the Active Directory Users and Computers program and right click on the Users folder. Select New -> Group. Give the group a name. For this scenario, we will be creating 2 groups named IT and HR. For now we will leave all options to its default and click OK.

[![image.png](http://192.168.1.119/uploads/images/gallery/2024-01/scaled-1680-/cWelzkU8rWzN1FBR-image.png)](http://192.168.1.119/uploads/images/gallery/2024-01/cWelzkU8rWzN1FBR-image.png)

[![image.png](http://192.168.1.119/uploads/images/gallery/2024-01/scaled-1680-/Xs71JD2EzARHubor-image.png)](http://192.168.1.119/uploads/images/gallery/2024-01/Xs71JD2EzARHubor-image.png)

Now that we have 2 newly created groups, lets also create some new users to add them to these group. Right click on the Users folder and go to New -> User. Fill in the information and click Next to set a password for the user.

<details>

<summary>For the sake of your sanity, you might want to disable complex password requirement policy temporarily</summary>

To disable the Password Complexity Requirement, log into the DC as a domain admin user (local Administrator account in my case).

Open Group Policy Management and navigate to the following options:<br>

[![image.png](http://192.168.1.119/uploads/images/gallery/2024-01/scaled-1680-/mRtggBnl2rxgUgk3-image.png)](http://192.168.1.119/uploads/images/gallery/2024-01/mRtggBnl2rxgUgk3-image.png)

This will open up a Group Policy Management Editor window. Navigate through the menu until you reach **Password Policy**. Change the Policy Setting for **Password must met complexity requirements** from Enabled to Disabled. Restart the server for the change to take place.<br>

[![image.png](http://192.168.1.119/uploads/images/gallery/2024-01/scaled-1680-/fKsqUxE6ocLjkno5-image.png)](http://192.168.1.119/uploads/images/gallery/2024-01/fKsqUxE6ocLjkno5-image.png)

You can turn it back on later when needed.

</details>

[![image.png](http://192.168.1.119/uploads/images/gallery/2024-01/scaled-1680-/WCMzZT7nSkVVX3OZ-image.png)](http://192.168.1.119/uploads/images/gallery/2024-01/WCMzZT7nSkVVX3OZ-image.png)

[![image.png](http://192.168.1.119/uploads/images/gallery/2024-01/scaled-1680-/a7ipgML1dSEqPqJH-image.png)](http://192.168.1.119/uploads/images/gallery/2024-01/a7ipgML1dSEqPqJH-image.png)

[![image.png](http://192.168.1.119/uploads/images/gallery/2024-01/scaled-1680-/0PhbuJtx2Lqzze7A-image.png)](http://192.168.1.119/uploads/images/gallery/2024-01/0PhbuJtx2Lqzze7A-image.png)

Repeating the steps above, I have created 3 users account.

[![image.png](http://192.168.1.119/uploads/images/gallery/2024-01/scaled-1680-/8nPDHFj2r4uHQUfj-image.png)](http://192.168.1.119/uploads/images/gallery/2024-01/8nPDHFj2r4uHQUfj-image.png)

Now, lets assign the users to the groups according to this table:

| User        | Group  |
| ----------- | ------ |
| Ivan Christ | HR     |
| John Smith  | IT     |
| Sam Walker  | HR, IT |

Right click on each of the user and select Add to a group option.

[![image.png](http://192.168.1.119/uploads/images/gallery/2024-01/scaled-1680-/hrIi30YxGCdLABGS-image.png)](http://192.168.1.119/uploads/images/gallery/2024-01/hrIi30YxGCdLABGS-image.png)

Type "HR" into the textbox and select Check Names. The text "HR" will have an underline, meaning that the object named HR exists on this santaa.com domain. If you have a typo, it will open up a new window telling you that the object is not found within this domain.

Do so for the rest of the user. To add a user to multiple group, you can repeat the add group process multiple time until you have that user into all the groups you wanted.

[![image.png](http://192.168.1.119/uploads/images/gallery/2024-01/scaled-1680-/vkZtIdYdP9OT23Sr-image.png)](http://192.168.1.119/uploads/images/gallery/2024-01/vkZtIdYdP9OT23Sr-image.png)

To confirm you added the user to groups, open the Properties window for the user and look under Member Of tab.

[![image.png](http://192.168.1.119/uploads/images/gallery/2024-01/scaled-1680-/QJDh3YMpTIIdDrYW-image.png)](http://192.168.1.119/uploads/images/gallery/2024-01/QJDh3YMpTIIdDrYW-image.png)

Alternatively, you can also add groups for this user in this tab.

***

## **Local Security Policy**

Now that we have successfully created groups, users and assigned users to groups, let's grant some permission.

A policy that is set on the group will be applied to all members in that group. So there is no need to configure the permission for all users in that group one by one.

In this scenario, we will allow our IT department to log in into our DC machine.

By default, all of our newly created user do not have permission to log in into the DC. When we try to sign in with as user jsmith, we will get an error like this:

[![image.png](http://192.168.1.119/uploads/images/gallery/2024-01/scaled-1680-/4PRdaFHIqvRIcinR-image.png)](http://192.168.1.119/uploads/images/gallery/2024-01/4PRdaFHIqvRIcinR-image.png)

***

### **Granting DC Sign In Permission for IT Department**

Sign back into the DC with a domain admin account and open up Group Policy Management window. Click on Edit on Default Domain Policy to open up the Policy Editor window.

[![image.png](http://192.168.1.119/uploads/images/gallery/2024-01/scaled-1680-/U5vNTh19yFHjlkFv-image.png)](http://192.168.1.119/uploads/images/gallery/2024-01/U5vNTh19yFHjlkFv-image.png)

Navigate to the **User Rights Assignment** under **Local Policies** and look for **Allow log on locally.**

[![image.png](http://192.168.1.119/uploads/images/gallery/2024-01/scaled-1680-/z8YuVbux2qqsFSl1-image.png)](http://192.168.1.119/uploads/images/gallery/2024-01/z8YuVbux2qqsFSl1-image.png)

Right click on the policy and open up the **Properties** window. Add the IT group into this policy.

[![image.png](http://192.168.1.119/uploads/images/gallery/2024-01/scaled-1680-/3uY0EIqVvEXq6KZK-image.png)](http://192.168.1.119/uploads/images/gallery/2024-01/3uY0EIqVvEXq6KZK-image.png)

[![image.png](http://192.168.1.119/uploads/images/gallery/2024-01/scaled-1680-/zdL3IvbY2iy1wl1c-image.png)](http://192.168.1.119/uploads/images/gallery/2024-01/zdL3IvbY2iy1wl1c-image.png)

Now that the IT group is added, click on "Apply" and restart the server.

[![image.png](http://192.168.1.119/uploads/images/gallery/2024-01/scaled-1680-/YTkXqJgsXI6YjLJg-image.png)](http://192.168.1.119/uploads/images/gallery/2024-01/YTkXqJgsXI6YjLJg-image.png)

After the server has restarted, try sign in as user jsmith again. We are now able to sign in to the DC. This will work for user Sam Walker too since he is also added to the IT group.

[![image.png](http://192.168.1.119/uploads/images/gallery/2024-01/scaled-1680-/Tk0p0Q6XwT08JItt-image.png)](http://192.168.1.119/uploads/images/gallery/2024-01/Tk0p0Q6XwT08JItt-image.png)

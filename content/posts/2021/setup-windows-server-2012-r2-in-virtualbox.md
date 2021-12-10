---
title: "How to Setup Windows Server 2012 R2 in VirtualBox?"
date: 2021-01-06T00:00:00+05:30
draft: false
author: "Daniccan"
categories:
  - Windows
  - Active Directory
tags:
  - VirtualBox
  - Windows-Server
  - Active-Directory
  - LDAP
---

## Posts in the Setting Up Active Directory Series

- [How to Setup Windows Server 2012 R2 in VirtualBox?](.)

## Pre-requisites

1. Download & Install [VirtualBox](https://www.virtualbox.org/wiki/Downloads).
2. Download the ISO of [Windows Server 2012 R2](https://www.microsoft.com/en-in/evalcenter/evaluate-windows-server-2012-r2).

## Steps

1. Open `VirtualBox` and click on `New` button in the menu bar.

{{< figure src="/posts/2021/setup-windows-server-2012-r2-in-virtualbox/Step_1.png" alt="Oracle VM VirtualBox Manager" width="600px" >}}

2. Enter the `Name` for the Virtual Machine and select `Windows 2012 (64-bit)` from the `Version` dropdown. Click on `Continue` to proceed.

{{< figure src="/posts/2021/setup-windows-server-2012-r2-in-virtualbox/Step_2.png" alt="Name and operating system" width="600px" >}}

3. The default memory allocation is equal to the recommended memory size. We can leave the default value as it is and click on `Continue`.

{{< figure src="/posts/2021/setup-windows-server-2012-r2-in-virtualbox/Step_3.png" alt="Memory size" width="600px" >}}

4. Select the `Create a virtual hard disk now` option and click on `Create`.

{{< figure src="/posts/2021/setup-windows-server-2012-r2-in-virtualbox/Step_4.png" alt="Hard disk" width="600px" >}}

5. Select the `VDI (VirtualBox Disk Image)` option and click on `Continue`.

{{< figure src="/posts/2021/setup-windows-server-2012-r2-in-virtualbox/Step_5.png" alt="Hard disk file type" width="600px" >}}

6. Select the `Dynamically allocated` option and click on `Continue`.

{{< figure src="/posts/2021/setup-windows-server-2012-r2-in-virtualbox/Step_6.png" alt="Storage on physical hard disk" width="600px" >}}

7. Browse and select the file location where you want to save the `.vdi` file and allocate the hard disk size for the same. Click on `Create` to proceed. This completes the creation of the Virtual Machine.

{{< figure src="/posts/2021/setup-windows-server-2012-r2-in-virtualbox/Step_7.png" alt="File location and size" width="600px" >}}

8. In `Settings`, under `Storage`, click on `Add disk`.

{{< figure src="/posts/2021/setup-windows-server-2012-r2-in-virtualbox/Step_8.png" alt="Storage" width="600px" >}}

9. Click on `Add` and select the Windows Server ISO image from your file system. Click `Choose` and click `OK` on the subsequent screens.

{{< figure src="/posts/2021/setup-windows-server-2012-r2-in-virtualbox/Step_9.png" alt="Select ISO image" width="600px" >}}

10. Start the Virtual Machine by selecting `Normal Start` from the list of `Start` options.

{{< figure src="/posts/2021/setup-windows-server-2012-r2-in-virtualbox/Step_10.png" alt="Normal Start of VM" width="600px" >}}

11. Select the ISO image from the list of dropdown options and click on `Start`.

{{< figure src="/posts/2021/setup-windows-server-2012-r2-in-virtualbox/Step_11.png" alt="Start by selecting ISO image" width="600px" >}}

12. Leave the defaults as it is in the language and time selection screen and click on `Next`.

{{< figure src="/posts/2021/setup-windows-server-2012-r2-in-virtualbox/Step_12.png" alt="Windows Setup - Language and Time selection" width="600px" >}}

13. Click on `Install now` to proceed.

{{< figure src="/posts/2021/setup-windows-server-2012-r2-in-virtualbox/Step_13.png" alt="Windows Setup - Install now" width="600px" >}}

14. Select the `Standard Evaluation` option with a GUI interface and click on `Next`.

{{< figure src="/posts/2021/setup-windows-server-2012-r2-in-virtualbox/Step_14.png" alt="Windows Setup - Select Standard Evaluation" width="600px" >}}

15. Accept the license terms by checking the `I accept the license terms` checkbox and click on `Next`.

{{< figure src="/posts/2021/setup-windows-server-2012-r2-in-virtualbox/Step_15.png" alt="Windows Setup - License terms" width="600px" >}}

16. Select the `Custom: Install Windows only (advanced)` option to do a fresh install of the Windows Server on the Virtual Machine.

{{< figure src="/posts/2021/setup-windows-server-2012-r2-in-virtualbox/Step_16.png" alt="Windows Setup - Custom installation of Windows" width="600px" >}}

17. Leave the defaults as it is for disk allocation and click on `Next`.

{{< figure src="/posts/2021/setup-windows-server-2012-r2-in-virtualbox/Step_17.png" alt="Windows Setup - Disk partition" width="600px" >}}

18. Windows will start getting installed on the Virtual Machine now. 

{{< figure src="/posts/2021/setup-windows-server-2012-r2-in-virtualbox/Step_18.png" alt="Windows Setup - Installation steps" width="600px" >}}

19. Once installation is completed, Windows will restart automatically.

{{< figure src="/posts/2021/setup-windows-server-2012-r2-in-virtualbox/Step_19.png" alt="Windows Setup - Auto restart" width="600px" >}}

20. Post restart, enter and confirm the `Password` for the `Administrator` user.

{{< figure src="/posts/2021/setup-windows-server-2012-r2-in-virtualbox/Step_20.png" alt="Windows Setup - Update Password" width="600px" >}}

21. Enter `Ctrl + Alt + Delete` to sign in to the Windows Server. On VirtualBox, this can be done by sending a keyboard input from the tools option.

{{< figure src="/posts/2021/setup-windows-server-2012-r2-in-virtualbox/Step_21.png" alt="Windows Setup - Ctrl + Alt + Del" width="600px" >}}

22. Enter the password of the `Administrator` User and proceed.

{{< figure src="/posts/2021/setup-windows-server-2012-r2-in-virtualbox/Step_22.png" alt="Windows Setup - Login" width="600px" >}}

23. Windows Server 2012 R2 is successfully setup on your Virtual Machine available via Oracle VirtualBox.

{{< figure src="/posts/2021/setup-windows-server-2012-r2-in-virtualbox/Step_23.png" alt="Windows Setup - Login successful" width="600px" >}}

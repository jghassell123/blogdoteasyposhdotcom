---
layout: post
title: "One Command Deploys A New Active Directory Infrastructure"
date: 2015-10-13 13:31:09 -0400
comments: true
categories: 
---
For the next few weeks I am going to highlight some useful scripts that you can use today no matter how experienced or how much of a novice you are at PowerShell. These scripts are immediately practical and yet by taking a look at the structure of the script and how we are using the facilities of the PowerShell scripting language, we can learn a lot about how to write our own scripts.

For a new project I've been building out a new lab infrastructure and as part of that I need to set up and deploy a totally new test environment consisting of Windows Server 2012 R2 and Exchange 2013. In order to be able to script this process as much as I can, I've been trying to do everything installation-wise through PowerShell so that I can build up a decent automation library that will let me consistently recreate this environment if necessary with just a few keystrokes.

Step one in this process, of course, was to set up a new Active Directory (AD) forest and domain controller. I installed Windows Server 2012 R2 on a new virtual machine and then as soon as I entered the password for the Administrator account, I opened a PowerShell prompt and used the following command:

<code>Install-ADDSForest
-CreateDnsDelegation:$false `
-DatabasePath "C:\Windows\NTDS" `
-DomainMode "Win2012R2" `
-DomainName "corp.salt-rose.com" `
-DomainNetbiosName "SALTROSE" `
-ForestMode "Win2012R2" `
-InstallDns:$true `
-LogPath "C:\Windows\NTDS" `
-NoRebootOnCompletion:$false `
-SysvolPath "C:\Windows\SYSVOL" `
-Force:$true</code>

Enter that at the prompt and you will be prompted to enter the Directory Services Restore Mode password twice–this is a password you are creating, so choose wisely. Then it took about three minutes and I was signed out and the new forest and domain was ready. Pretty cool.
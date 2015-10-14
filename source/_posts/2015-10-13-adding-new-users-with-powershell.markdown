---
layout: post
title: "Adding New Users with PowerShell"
date: 2015-10-13 14:07:57 -0400
comments: true
categories: 
---
<i>For the next few weeks I am going to highlight some useful scripts that you can use today no matter how experienced or how much of a novice you are at PowerShell. These scripts are immediately practical and yet by taking a look at the structure of the script and how we are using the facilities of the PowerShell scripting language, we can learn a lot about how to write our own scripts.</i>

Have you ever had a batch of users you needed to create accounts for, but you did not want to page through the wizards in Active Directory Users & Computers? This kind of rote, repetitive task is exactly what Windows PowerShell is designed to handle.

<code>Import-Module ActiveDirectory
Import-Csv "C:\powershell\users.csv" | ForEach-Object {
$userPrincipal = $_."samAccountName" + "@yourdomain.local"
New-ADUser -Name $_.Name `
-Path $_."ParentOU" `
-SamAccountName $_."samAccountName" `
-UserPrincipalName $userPrincipal `
-AccountPassword (ConvertTo-SecureString "cheeseburgers4all" -AsPlainText -Force) `
-ChangePasswordAtLogon $true `
-Enabled $true
Add-ADGroupMember "Office Users" $_."samAccountName";
}</code>

In this script, we use the Import-Csv cmdlet, which knows how to read .CSV formatted files. We tell the Import-CSV cmdlet that each row of the CSV located in C:\powershell called users.csv contains information in three columns – the Name of the user, the samAccountName of the user which is basically the login ID for the user, and the organizational unit (OU) of Active Directory that the user needs to live in – and that we are using the columns samAccount Name to create the login ID for the user by marrying the value that lives in that column with the string "@yourdomain.local" to complete the user principal name (UPN).

From there, we loop through the file using ForEach-Object and send that assembled string (which is stored in the PowerShell variable name $userPrincipal).

We assign the default password to each user "cheeseburgers4all" and then set the Active Directory flag to require the user to change the password at first logon. At the end of the script, we then add all of these accounts to the Active Directory security group called Office Users.
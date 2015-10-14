---
layout: post
title: "Track Failed Logon Events for Active Directory Domain Controllers"
date: 2015-10-13 14:18:53 -0400
comments: true
categories: 
---
For the next few weeks I am going to highlight some useful scripts that you can use today no matter how experienced or how much of a novice you are at PowerShell. These scripts are immediately practical and yet by taking a look at the structure of the script and how we are using the facilities of the PowerShell scripting language, we can learn a lot about how to write our own scripts.

On a mailing list I am a member of, recently someone posted that they were searching for a simple way to see failed logon events by username from their two domain controllers, both running Windows Server 2012 and IIS 7.5. They wanted the username, IP address of machine that was attempted to be accessed, and source IP address of the request.

PowerShell is great for this. Here’s a sample.

<code>Get-EventLog -logname Security -after 1/1/2015 -before 6/30/2015 | where {$_.eventID -eq 4625} | select MachineName,EventID,Source,TimeGenerated,EntryType,Message | Out-File C:\failedlogons.txt</code>

We're using the Get-EventLog cmdlet to look at only the Security event log within a specific time window (you can customize that of course) and then using the where-object cmdlet to select event 4625, which is the standard Windows event ID for a failed logon. From that object, we are grabbing the name of the machine, the event ID (4625), the source, date and time, and any message, and then we are using the Out-File cmdlet to write just that narrowed down, selected information to a file called C:\failedlogons.txt.

To automate this, you'd run it once a day by putting it into Scheduled Tasks and then slowly build your outfile with the data which you could then query by importing it into Excel and managing it there.

<i>hat tip to Alex Keller of Stanford University</i>
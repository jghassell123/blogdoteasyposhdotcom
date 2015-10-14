---
layout: post
title: "Warn When a Folder Gets Too Big or Exceeds a Certain Size"
date: 2015-10-13 14:16:13 -0400
comments: true
categories: 
---
For the next few weeks I am going to highlight some useful scripts that you can use today no matter how experienced or how much of a novice you are at PowerShell. These scripts are immediately practical and yet by taking a look at the structure of the script and how we are using the facilities of the PowerShell scripting language, we can learn a lot about how to write our own scripts.

Do you have certain software that tends to write a lot of data to disk but isn't great about cleaning it up? Is there one specific directory you want to monitor and be alerted to once that directory's size grows above a certain threshold? This script does that: it looks at a specific path, monitors the size of a directory, and if it grows above a threshold value, it fires a warning message to the server console session and also writes an event (event ID 9990, to be specific) to the Application event log.

<code>Add-Type -Assembly System.Windows.Forms

New-EventLog -LogName Application -Source "Folder Size PowerShell Script"

$Threshold = 1024 * 1024 * 1024

$Path = "c:\directorytomonitor"

$WarningMessage = "The size of folder $Path exceeds the space threshold that has been established."

$scratchpad = Get-ChildItem $Path | Measure-Object -Property Length -sum

$Value = $scratchpad.sum

if( $Value -gt $Threshold) {
Write-Warning $Message
Write-EventLog -LogName Application -Source "Folder Size PowerShell Script" -EntryType Information -EventID 9990 -Message $WarningMessage
$ScriptResult = [System.Windows.Forms.Messagebox]::Show($WarningMessage)
}
</code>

This one is fairly straightforward to walk through. First, we set up a bunch of variables to hold the directory we want to monitor, the threshold at which we want to be alerted, the message to be written out in the event the threshold is breached, and then the GCI to grab the size property from the path object. There is then simple logic equation: if that value is greater than threshold, then we'll fire a warning message and write a new event log entry. Pretty easy.

<i>hat tip to Paul Wolfson of Dallas Legal Technology</i>
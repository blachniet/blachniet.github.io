---
title: "Event Log Email Alerts with PowerShell"
date: "2013-07-28"
categories:
- "site-updates"
tags:
- "gmail"
- "powershell"
- "smtp"
aliases:
- "/blog/event-log-email-alerts-with-powershell/"
blachnietWordPressExport: true
---

_Source on GitHub: [https://github.com/blachniet/blachniet-psutils](https://github.com/blachniet/blachniet-psutils)_

Have you ever wanted to send out email alerts when an particular event appears in the Windows Event Log? The Task Scheduler provides the ability to send out emails when an event is logged, but it doesn't allow for SMTP servers that require any sort of authentication. I wanted to send the emails from a Gmail , so created a PowerShell script to handle this.

The script takes in quite a few required parameters in order to make it more flexible and reusable, so it may seem a little complicated at first. The mandatory parameters are `Source`, `SmtpUser`, `SmtpPassword`, `MailFrom`, and `MailTo`. All the other parameters have sensible defaults for sending emails through Gmail. You could customize these to be able to send email through some other provider if you needed to. Use `Get-Help` to get more information about all the parameters as well as some example usages.

```powershell
Get-Help Send-EventEntryEmail --detailed
```

A scheduled task is still used to fire off the script, but the script handles getting the event entry information and sending off the email. Just create a new scheduled task with a trigger set to `On an event`. You can configure the trigger to watch for particular content in an event, events from a particular source, etc.

The implementation is actually pretty simple. To start off, I pull the latest event entries with `Get-EventLog`, and filter the results based on the script parameters (`Source`, `Newest`, `LogName`).

```powershell
# Get the event entries.
$eventEntries = Get-EventLog -LogName $LogName -Source $Source -Newest $Newest -EntryType $EntryType
```

`$eventEntries` is an array of [System.Diagnostics.EventLogEntry](http://msdn.microsoft.com/en-us/library/system.diagnostics.eventlogentry.aspx). I then build a html table row for each of these entries and include the time the event was generated, the type of event, and the message associated with the event.

```powershell
# Create a table row for each entry.
$rows = ""
foreach ($eventEntry in $eventEntries){
    $rows += @"
    <tr>
        <td style="text-align: center; padding: 5px;">$($eventEntry.TimeGenerated)</td>
        <td style="text-align: center; padding: 5px;">$($eventEntry.EntryType)</td>
        <td style="padding: 5px;">$($eventEntry.Message)</td>
    </tr>
"@
}
```

To finish things up, just create a new `MailMessage` and `SmtpClient` and send of the email.

```powershell
# Create the email.
$email = New-Object System.Net.Mail.MailMessage( $MailFrom , $MailTo )
$email.Subject = $Subject
$email.IsBodyHtml = $true
$email.Body = @"
<table style="width:100%;border">
    <tr>
        <th style="text-align: center; padding: 5px;">Time</th>
        <th style="text-align: center; padding: 5px;">Type</th>
        <th style="text-align: center; padding: 5px;">Message</th>
    </tr>

$rows

</table>
"@

# Send the email.
$SMTPClient=New-Object System.Net.Mail.SmtpClient( $SmtpServer , $SmtpPort )
$SMTPClient.EnableSsl=$true
$SMTPClient.Credentials=New-Object System.Net.NetworkCredential( $SmtpUser , $SmtpPassword );
$SMTPClient.Send( $email )
```

Hopefully others will find this script useful. Feel free to customize the script if you want. You can find it in my GitHub project [https://github.com/blachniet/blachniet-psutils](https://github.com/blachniet/blachniet-psutils).

# Hunt

**All tests were conducted using the following account to help replicate minimum domain user privileges.**

```
PS C:\Users\g.white> net user /domain g.white
The request will be processed at a domain controller for domain hacklab.local.

User name                    g.white
Full Name
Comment
User's comment
Country/region code          000 (System Default)
Account active               Yes
Account expires              Never

Password last set            07/06/1841 11:29:03
Password expires             Never
Password changeable          08/06/21841 11:29:03
Password required            Yes
User may change password     Yes

Workstations allowed         All
Logon script
User profile
Home directory
Last logon                   07/06/1841 11:29:11

Logon hours allowed          All

Local Group Memberships
Global Group memberships     *Domain Users
The command completed successfully.

PS C:\Users\g.white>
```

**Local hosts netsetup log file**

Review The Netsetup log file on the local host which contains information for helping to troubleshooting domain joining issue.
The log file contains the host build information, the full domain name and domain controller’s host name. 

```
type C:\Windows\debug\NetSetup.LOG
```

**Demo**

```
PS C:\Users\g.white> type C:\Windows\debug\NetSetup.LOG
04/15/1748 13:12:46:983 -----------------------------------------------------------------
04/15/1748 13:12:46:983 NetpDoDomainJoin
04/15/1748 13:12:46:983 NetpDoDomainJoin: using new computer names
04/15/1748 13:12:46:983 NetpDoDomainJoin: NetpGetNewMachineName returned 0x0
04/15/1748 13:12:46:983 NetpMachineValidToJoin: 'WIN-60SQ84GOA6K'
04/15/1748 13:12:46:983         OS Version: 10.0
04/15/1748 13:12:46:983         Build number: 19045 (19041.vb_release.191206-1406)
04/15/1748 13:12:46:983         SKU: Windows 10 Enterprise Evaluation
04/15/1748 13:12:46:983         Architecture: 64-bit (AMD64)
04/15/1748 14:28:36:638 NetpDoDomainJoin
04/15/1748 14:28:36:638 NetpDoDomainJoin: using new computer names
04/15/1748 14:28:36:638 NetpDoDomainJoin: NetpGetNewMachineName returned 0x0
04/15/1748 14:28:36:638 NetpDoDomainJoin: NetpGetNewHostName returned 0x0
04/15/1748 14:28:36:638 NetpMachineValidToJoin: 'WIN-10-LAB'
04/15/1748 14:28:36:638         OS Version: 10.0
04/15/1748 14:28:36:638         Build number: 19045 (19041.vb_release.191206-1406)
04/15/1748 14:28:36:638         SKU: Windows 10 Enterprise Evaluation
04/15/1748 14:28:36:638         Architecture: 64-bit (AMD64)
04/15/1748 14:28:36:654 NetpMachineValidToJoin: status: 0x0
04/15/1748 14:28:36:654 NetpJoinDomain
04/15/1748 14:28:36:654         HostName: Win-10-lab
04/15/1748 14:28:36:654         NetbiosName: WIN-10-LAB
04/15/1748 14:28:36:654         Domain: hacklab.local
04/15/1748 14:28:36:654         MachineAccountOU: (NULL)
04/15/1748 14:28:36:654         Account: hacklab.local\g.white
04/15/1748 14:28:36:654         Options: 0x425

```

**Hunt for the keyword of password within the following documents formats *.ini,*.txt,*.doc,*.docx,*.xml,*.config recursively across C:\ drive.**

```
Get-ChildItem -Path C:\ -Recurse -Include *.ini,*.txt,*.doc,*.docx,*.xml,*.config -File -ErrorAction SilentlyContinue | ForEach-Object { Select-String -Pattern 'password' -Path $_.FullName -ErrorAction SilentlyContinue } | ForEach-Object { Write-Output "File: $($_.Path)`nMatch: $($_.Line)" }
```

**Demo**

```
PS C:\Users\g.white> Get-ChildItem -Path C:\ -Recurse -Include *.ini,*.txt,*.doc,*.docx,*.xml,*.config -File -ErrorAction SilentlyContinue | ForEach-Object { Select-String -Pattern 'password' -Path $_.FullName -ErrorAction SilentlyContinue } | ForEach-Object { Write-Output "File: $($_.Path)`nMatch: $($_.Line)" }
File: C:\Program Files\Common Files\microsoft shared\ink\Alphabet.xml
Match:       <phrase>Du kan starte dit password med *.</phrase>
File: C:\Program Files\VMware\VMware Tools\open_source_licenses.txt
Match: source code form), and must require no special password or key for
PS C:\Users\g.white>
```


**Hunt for UNC paths within the following documents formats *.ini,*.txt,*.doc,*.docx,*.xml,*.config recursively across C:\ drive.**

```
Get-ChildItem -Path C:\ -Recurse -Include *.ini,*.txt,*.doc,*.docx,*.xml,*.config -File -ErrorAction SilentlyContinue | ForEach-Object { Select-String -Pattern '\\\\[a-zA-Z0-9_.-]+\\[a-zA-Z0-9$_.-]+' -Path $_.FullName -ErrorAction SilentlyContinue } | ForEach-Object { Write-Output "File: $($_.Path)`nMatch: $($_.Line)" }
```

**Demo**

```
PS C:\Users\g.white> Get-ChildItem -Path C:\ -Recurse -Include *.ini,*.txt,*.doc,*.docx,*.xml,*.config -File -ErrorAction SilentlyContinue | ForEach-Object { Select-String -Pattern '\\\\[a-zA-Z0-9_.-]+\\[a-zA-Z0-9$_.-]+' -Path $_.FullName -ErrorAction SilentlyContinue } | ForEach-Object { Write-Output "File: $($_.Path)`nMatch: $($_.Line)" }
File: C:\Users\g.white\Desktop\Client_Tools\test.txt
Match: \\WIN-8HPLF8PSHC1\HR - Read access
File: C:\Users\g.white\Desktop\Client_Tools\test.txt
Match: \\WIN-8HPLF8PSHC1\IT - Read access
File: C:\Users\g.white\Desktop\Client_Tools\test.txt
Match: \\WIN-8HPLF8PSHC1\NETLOGON - Read access
PS C:\Users\g.white>
```

**Hunt for the keyword of password within the following documents formats *.ini,*.txt,*.doc,*.docx,*.xml,*.config recursively across network share.**

```
Get-ChildItem -Path \\hacklab.local\SYSVOL\hacklab.local -Recurse -Include *.ini,*.txt,*.doc,*.docx,*.xml,*.config -File -ErrorAction SilentlyContinue | ForEach-Object { Select-String -Pattern 'password' -Path $_.FullName -ErrorAction SilentlyContinue } | ForEach-Object { Write-Output "File: $($_.Path)`nMatch: $($_.Line)" }
```

**Demo**

```
PS C:\Users\g.white> Get-ChildItem -Path \\hacklab.local\SYSVOL\hacklab.local -Recurse -Include *.ini,*.txt,*.doc,*.docx,*.xml,*.config -File -ErrorAction SilentlyContinue | ForEach-Object { Select-String -Pattern 'password' -Path $_.FullName -ErrorAction SilentlyContinue } | ForEach-Object { Write-Output "File: $($_.Path)`nMatch: $($_.Line)" }
File: \\hacklab.local\SYSVOL\hacklab.local\scripts\Shares\Game1.txt
Match: password = fishandchips1
File: \\hacklab.local\SYSVOL\hacklab.local\scripts\Shares\Game2.txt
Match: please use this username Admin2 and password of Password!
File: \\hacklab.local\SYSVOL\hacklab.local\scripts\Shares\Script99.txt
Match: Password Pasmeup1
File: \\hacklab.local\SYSVOL\hacklab.local\scripts\Shares\Test2.txt
Match: Password:football
PS C:\Users\g.white>
```

**Hunt for UNC paths within the following documents formats *.ini,*.txt,*.doc,*.docx,*.xml,*.config recursively across network share.**

```
Get-ChildItem -Path \\hacklab.local\SYSVOL\hacklab.local -Recurse -Include *.ini,*.txt,*.doc,*.docx,*.xml,*.config -File -ErrorAction SilentlyContinue | ForEach-Object { Select-String -Pattern '\\\\[a-zA-Z0-9_.-]+\\[a-zA-Z0-9$_.-]+' -Path $_.FullName -ErrorAction SilentlyContinue } | ForEach-Object { Write-Output "File: $($_.Path)`nMatch: $($_.Line)" }
```

**Demo**

```
PS C:\Users\g.white> Get-ChildItem -Path \\hacklab.local\SYSVOL\hacklab.local -Recurse -Include *.ini,*.txt,*.doc,*.docx,*.xml,*.config -File -ErrorAction SilentlyContinue | ForEach-Object { Select-String -Pattern '\\\\[a-zA-Z0-9_.-]+\\[a-zA-Z0-9$_.-]+' -Path $_.FullName -ErrorAction SilentlyContinue } | ForEach-Object { Write-Output "File: $($_.Path)`nMatch: $($_.Line)" }
File: \\hacklab.local\SYSVOL\hacklab.local\scripts\Shares\Deep\In\The\Cave\Script_remove1.txt
Match: \\WIN-8HPLF8PSHC1\HR
File: \\hacklab.local\SYSVOL\hacklab.local\scripts\Shares\Deep\In\The\Cave\Script_remove1.txt
Match: \\WIN-8HPLF8PSHC1\IT
File: \\hacklab.local\SYSVOL\hacklab.local\scripts\Shares\Game1.txt
Match: try this share \\happy01\test\
File: \\hacklab.local\SYSVOL\hacklab.local\scripts\Config.INI
Match: \\WIN-10-LAB\C$
File: \\hacklab.local\SYSVOL\hacklab.local\scripts\Config.INI
Match: \\WIN-10-LAB-2\Fox
PS C:\Users\g.white>
```




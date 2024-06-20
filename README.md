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

**Local log file for Windows Malicious Software Removal Tool (Defender)**

```
type C:\Windows\debug\mrt.log
```

**Demo**

```
PS C:\Users\g.white> type C:\Windows\debug\mrt.log

---------------------------------------------------------------------------------------
Microsoft Windows Malicious Software Removal Tool v5.123, (build 5.123.24040.1001)
Started On Wed Apr 17 15:42:27 1748

Engine: 1.1.24020.9
Signatures: 1.407.485.0
MpGear: 1.1.16330.1
Run Mode: Scan Run From Windows Update

Results Summary:
----------------
No infection found.
Successfully Submitted Heartbeat Report
Microsoft Windows Malicious Software Removal Tool Finished On Wed Apr 17 15:45:13 1748
```

**Windows explorer search strings, mount a share with windows explorer and use these in the search option to hunt for keywords within documents.**

```
content:"password“
content:"cred"
content:"password" AND *.txt
content:"password" AND *.xls
content:"password" AND *.bat
content:"password" AND *.ini
```

**Windows explorer search string to hunt for network shares within documents.**

```
content:"\\"
```


**Combing Windows explorer search strings, mount a share with windows explorer and use these in the search option to hunt for keywords within documents and file names.**

```
Groups.xml OR content:"password" OR password
```

**Mount a remote share**

```
pushd \\hacklab.local\SYSVOL\hacklab.local
```

**Demo**

```
PS C:\Users\g.white> pushd \\hacklab.local\SYSVOL\hacklab.local
PS Microsoft.PowerShell.Core\FileSystem::\\hacklab.local\SYSVOL\hacklab.local>
```

**Search for file names, formats and locations on local or remote share, execute the command in CMD.**

```
dir /s *.xml *.ini .*bat > C:\Users\g.white\Desktop\Results1.txt
```

**Demo**

```
Microsoft Windows [Version 10.0.19045.4291]
(c) Microsoft Corporation. All rights reserved.

C:\Users\g.white>pushd \\hacklab.local\SYSVOL\hacklab.local

Y:\hacklab.local>dir /s *.xml *.ini .*bat > C:\Users\g.white\Desktop\Results1.txt

Y:\hacklab.local>type C:\Users\g.white\Desktop\Results1.txt
 Volume in drive Y has no label.
 Volume Serial Number is A45A-D553

 Directory of Y:\hacklab.local\Policies\{31B2F340-016D-11D2-945F-00C04FB984F9}

15/04/2024  12:10                22 GPT.INI
               1 File(s)             22 bytes

 Directory of Y:\hacklab.local\Policies\{6AC1786C-016F-11D2-945F-00C04fB984F9}

08/05/2024  10:29                22 GPT.INI
               1 File(s)             22 bytes

 Directory of Y:\hacklab.local\Policies\{6AC1786C-016F-11D2-945F-00C04fB984F9}\MACHINE

12/04/2024  13:37               824 Groups.xml
               1 File(s)            824 bytes
```

**Hunt for keywords within defined file formats using CMD**

```
findstr /si password *.bat *.xml *.ini *.txt > C:\Users\g.white\Desktop\findstr1.txt
```

**Demo**

```
Y:\hacklab.local>findstr /si password *.bat *.xml *.ini *.txt > C:\Users\g.white\Desktop\Output\findstr1.txt
The system cannot find the path specified.

Y:\hacklab.local>findstr /si password *.bat *.xml *.ini *.txt > C:\Users\g.white\Desktop\findstr1.txt

Y:\hacklab.local>
Y:\hacklab.local>
Y:\hacklab.local>type C:\Users\g.white\Desktop\findstr1.txt
scripts\Config.INI:password called Hello@1
scripts\Pingy\Test.txt:This is a test as it contains a password called fishhead1scripts\Shares\Brandon_DiCam\Startup.bat:    [string]$Password = 'Passw0rd!'
```

**Same as above but highlight matched filenames in red**

```
findstr /A:4 /spin "passw" *.txt*
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

**VBA Script to be used with office documents to hunt for key words across a defined network share, tweak as required.**

```
Sub SearchKeywordInNetworkFiles()
    Dim searchFolder As String
    Dim textToFind As String
    Dim fileExtensions As Variant
    Dim resultMessage As String
    Dim outputFilePath As String
    
    ' Add your keyword to search here.
    textToFind = "password"
    
    ' Add the network share folder path to search here.
    searchFolder = "\\hacklab.local\SYSVOL\hacklab.local\scripts"
    
    ' Add any extra file extensions to search here or keep the defaults below
    fileExtensions = Array("*.docx", "*.bat", "*.txt", "*.ini", "*.xml")
    
    resultMessage = "Keyword '" & textToFind & "' found in the following files:" & vbCrLf & vbCrLf
    
    ' Add your output file path here.
    outputFilePath = "C:\Users\g.white\Desktop\OutRun\Results.txt"
    
    Call RecursiveFileSearch(searchFolder, textToFind, fileExtensions, resultMessage)
    
    MsgBox resultMessage
    
    Call WriteResultsToFile(outputFilePath, resultMessage)
End Sub

Sub RecursiveFileSearch(folderPath As String, textToFind As String, fileExtensions As Variant, ByRef resultMessage As String)
    Dim fso As Object
    Dim folder As Object
    Dim subFolder As Object
    Dim file As Object
    Dim filePath As String
    Dim fileName As String
    Dim fileContent As String
    Dim doc As Object
    Dim ts As Object
    Dim i As Integer
    
    Set fso = CreateObject("Scripting.FileSystemObject")
    
    On Error GoTo ErrorHandler
    Set folder = fso.GetFolder(folderPath)
    
    For i = LBound(fileExtensions) To UBound(fileExtensions)
        fileName = Dir(folderPath & "\" & fileExtensions(i))
        
        Debug.Print "Searching for files in: " & folderPath & "\" & fileExtensions(i)
        
        Do While fileName <> ""

            filePath = folderPath & "\" & fileName
            
            Debug.Print "Processing file: " & filePath
            
            If fileExtensions(i) = "*.docx" Then

                Set doc = GetObject(filePath)
                
                fileContent = doc.Content.Text
                
                doc.Close SaveChanges:=False
            Else

                Set file = fso.GetFile(filePath)
                Set ts = file.OpenAsTextStream(1)
                fileContent = ts.ReadAll
                ts.Close
            End If
            

            If InStr(1, fileContent, textToFind, vbTextCompare) > 0 Then

                resultMessage = resultMessage & filePath & vbCrLf
            End If
            
            fileName = Dir
        Loop
    Next i
    
    For Each subFolder In folder.SubFolders
        Call RecursiveFileSearch(subFolder.Path, textToFind, fileExtensions, resultMessage)
    Next subFolder
    
    Set folder = Nothing
    Set fso = Nothing
    Set file = Nothing
    Set ts = Nothing
    Exit Sub
    
ErrorHandler:
    MsgBox "Error: " & Err.Description & " in folder: " & folderPath
    Resume Next
End Sub

Sub WriteResultsToFile(outputFilePath As String, resultMessage As String)
    Dim fso As Object
    Dim outputFile As Object
    
    Set fso = CreateObject("Scripting.FileSystemObject")
    
    Set outputFile = fso.CreateTextFile(outputFilePath, True)
    
    outputFile.WriteLine resultMessage
    
    outputFile.Close
    
    Set outputFile = Nothing
    Set fso = Nothing
End Sub

```


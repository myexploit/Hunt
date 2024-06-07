# Hunt


Hunt for the keyword of password within the following documents formats *.ini,*.txt,*.doc,*.docx,*.xml,*.config recursively across C:\ drive.

```
Get-ChildItem -Path C:\ -Recurse -Include *.ini,*.txt,*.doc,*.docx,*.xml,*.config -File -ErrorAction SilentlyContinue | ForEach-Object { Select-String -Pattern 'password' -Path $_.FullName -ErrorAction SilentlyContinue } | ForEach-Object { Write-Output "File: $($_.Path)`nMatch: $($_.Line)" }
```

Demo

```
PS C:\Users\g.white> Get-ChildItem -Path C:\ -Recurse -Include *.ini,*.txt,*.doc,*.docx,*.xml,*.config -File -ErrorAction SilentlyContinue | ForEach-Object { Select-String -Pattern 'password' -Path $_.FullName -ErrorAction SilentlyContinue } | ForEach-Object { Write-Output "File: $($_.Path)`nMatch: $($_.Line)" }
File: C:\Program Files\Common Files\microsoft shared\ink\Alphabet.xml
Match:       <phrase>Du kan starte dit password med *.</phrase>
File: C:\Program Files\VMware\VMware Tools\open_source_licenses.txt
Match: source code form), and must require no special password or key for
PS C:\Users\g.white>
```


Hunt for UNC paths within the following documents formats *.ini,*.txt,*.doc,*.docx,*.xml,*.config recursively across C:\ drive.

```
Get-ChildItem -Path C:\ -Recurse -Include *.ini,*.txt,*.doc,*.docx,*.xml,*.config -File -ErrorAction SilentlyContinue | ForEach-Object { Select-String -Pattern '\\\\[a-zA-Z0-9_.-]+\\[a-zA-Z0-9$_.-]+' -Path $_.FullName -ErrorAction SilentlyContinue } | ForEach-Object { Write-Output "File: $($_.Path)`nMatch: $($_.Line)" }
```

Demo

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

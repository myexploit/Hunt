# Hunt



```
Get-ChildItem -Path C:\ -Recurse -Include *.ini,*.txt,*.doc,*.docx,*.xml,*.config -File -ErrorAction SilentlyContinue | ForEach-Object { Select-String -Pattern 'password' -Path $_.FullName -ErrorAction SilentlyContinue } | ForEach-Object { Write-Output "File: $($_.Path)`nMatch: $($_.Line)" }
```

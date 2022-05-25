# Profile

Profile file can be accessed via `$profile` variable:

```powershell
code $profile
```

Current contents of my `$profile` are as follows. Commands in comments are one time installation commands.

```powershell
# Install-Module -Name PowerShellGet -Repository PSGallery -Force
# Install-Module PSReadLine -AllowPrerelease -Force
Import-Module PSReadLine
Set-PSReadLineOption -PredictionSource History
Set-PSReadLineOption -PredictionViewStyle ListView

# Bash-like TAB for cd
Set-PSReadlineKeyHandler -Key Tab -Function Complete

# Bash-like Ctrl-d
Set-PSReadlineKeyHandler -Key Ctrl+d -Function DeleteCharOrExit

if ($env:WT_SESSION)
{
    # https://www.nerdfonts.com/ -> CascadiaCode font
    # Install-Module -Name Terminal-Icons -Repository PSGallery
    Import-Module -Name Terminal-Icons
}

# Aliases:
Set-Alias -Name "less" -Value "C:\Program Files\Git\usr\bin\less.exe"
Set-Alias -Name "head" -Value "C:\Program Files\Git\usr\bin\head.exe"
Set-Alias -Name "tail" -Value "C:\Program Files\Git\usr\bin\tail.exe"
Set-Alias -Name "xxd" -Value "C:\Program Files\Git\usr\bin\xxd.exe"
Set-Alias -Name "grep" -Value "C:\Program Files\Git\usr\bin\grep.exe"
Set-Alias -Name "uniq" -Value "C:\Program Files\Git\usr\bin\uniq.exe"
Set-Alias -Name "du" -Value "C:\Program Files\Git\usr\bin\du.exe"
Set-Alias -Name "wc" -Value "C:\Program Files\Git\usr\bin\wc.exe"

function which($name) 
{
    Get-Command $name | Select-Object -ExpandProperty Definition
}
```

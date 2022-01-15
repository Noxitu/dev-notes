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

if ($env:WT_SESSION)
{
    # https://www.nerdfonts.com/ -> CascadiaCode font
    # Install-Module -Name Terminal-Icons -Repository PSGallery
    Import-Module -Name Terminal-Icons
}

# Aliases:
Set-Alias -Name "less" -Value "C:\Program Files\Git\usr\bin\less.exe"

function which($name) 
{
    Get-Command $name | Select-Object -ExpandProperty Definition
}
```

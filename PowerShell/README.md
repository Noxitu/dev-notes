# Profile

Profile file can be accessed via `$profile` variable:

    code $profile

Current contents of my `$profile` are as follows. Commands in comments are one time installation commands.

    # Install-Module -Name PowerShellGet -Repository PSGallery -Force
    # Install-Module PSReadLine -AllowPrerelease -Force
    Import-Module PSReadLine
    Set-PSReadLineOption -PredictionSource History
    Set-PSReadLineOption -PredictionViewStyle ListView

    # Bash-like TAB for cd
    Set-PSReadlineKeyHandler -Key Tab -Function Complete
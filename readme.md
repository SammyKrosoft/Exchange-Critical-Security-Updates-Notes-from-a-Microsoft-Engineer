# Exchange Important Security Updates
*Updated 13th July 2021*

In this repository I would like to highlight important Exchange Server specific security updates as they are announced by Microsoft within the [Security Update Guide](https://msrc.microsoft.com/update-guide).

I will update this Readme as I add articles to this repository.

# Latest Exchange Server specific security issues

## [13 July 2021 Vulnerability issues page](https://github.com/SammyKrosoft/Exchange-Critical-Security-Updates-Notes-from-a-Microsoft-Engineer/blob/main/July-13-2021-Vulnerability.md)

# Older security issues

## [March 2021 vulnerability fixes (Hafnium attacks)](https://github.com/SammyKrosoft/Exchange-Critical-Security-Updates-Notes-from-a-Microsoft-Engineer/blob/main/March-2021-Hafnium-Attacks-Protection.md)

# TIP: Quick script to get your Exchange Servers exact version numbers

```powershell
$ExchangeServers = Get-ExchangeServer | Sort-Object Name
ForEach ($Server in $ExchangeServers) {
    Invoke-Command -ComputerName $Server.Name -ScriptBlock { Get-Command Exsetup.exe | ForEach-Object { $_.FileversionInfo } }
}
```

# Useful links (CTRL + Click to open in a new tab)

## [Exchange build numbers and all CUs download links](https://docs.microsoft.com/en-us/exchange/new-features/build-numbers-and-release-dates?view=exchserver-2019)

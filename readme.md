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

Sample output (my Lab):

```output
ProductVersion   FileVersion      FileName                            PSComputerName                    
--------------   -----------      --------                            --------------                    
15.01.2044.004   15.01.2044.004   C:\Program Files\Microsoft\Excha... E2016-01                          
15.01.2308.008   15.01.2308.008   C:\Program Files\Microsoft\Excha... E2016-02                          
```

## Compare the versions you get from the above with the numbers on the below link:

## [Exchange build numbers and all CUs download links](https://docs.microsoft.com/en-us/exchange/new-features/build-numbers-and-release-dates?view=exchserver-2019)

Example finding my CU level with the above example:

<img src = https://user-images.githubusercontent.com/33433229/125547107-9a243329-43fc-4435-b32b-8613d5fc1a74.png width = 600>

And the ovher one (15.01.2044.004):

<img src = "https://user-images.githubusercontent.com/33433229/125547168-b23686d5-9160-4961-ab57-8624369ac27b.png" width = 600



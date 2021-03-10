# Important information regarding Exchange Server (2010, 2013, 2016, 2019) 0-day exploits

On Tuesday 2nd March 2021, Microsoft released patches for multiple different on-premises Microsoft Exchange Server zero-day vulnerabilities that are being exploited by a nation-state affiliated group.  The vulnerabilities exist in on-premises Exchange Servers 2010, 2013, 2016, and 2019.  
 
Your Microsoft Customer Success Account Manager and Technical Support Teams will be engaging with your technical teams to assist in addressing this issue.  We wanted to ensure you were aware of the situation and would ask that you help drive immediate remediation steps.
 
For on-premises Exchange Servers, we ask that you direct your teams to start immediate action to assess your Exchange infrastructure and patch vulnerable servers, with the first priority being servers which are accessible from the Internet (e.g., servers publishing Outlook on the web/OWA and ECP).  To patch these vulnerabilities, you should move to the latest Exchange Cumulative Updates and then install the relevant security updates on each Exchange Server.  You can use the Exchange Server Health Checker script, which can be downloaded from GitHub (use the latest release). Running this script will tell you if you are behind on your on-premises Exchange Server updates (note that the script does not support Exchange Server 2010). 
 
We also recommend that your security team assess whether or not the vulnerabilities were being exploited by using the Indicators of Compromise we shared here - 
https://www.microsoft.com/security/blog/2021/03/02/hafnium-targeting-exchange-servers/

 
 
We are committed to working with you through this issue.  Your Microsoft account and support teams have been fully mobilized.  Please let me know if you need additional help.

|Information to assist you and your teams|
|---|
|[March 2, 2021 Security Update Release - Release Notes - Security Update Guide - Microsoft](https://msrc.microsoft.com/update-guide/releaseNote/2021-Mar)|
|[CVE-2021-26412](https://msrc.microsoft.com/update-guide/en-US/vulnerability/CVE-2021-26412)|
|[CVE-2021-26854](https://msrc.microsoft.com/update-guide/en-US/vulnerability/CVE-2021-26854)|
|[CVE-2021-26855](https://msrc.microsoft.com/update-guide/en-US/vulnerability/CVE-2021-26855)|
|[CVE-2021-26857](https://msrc.microsoft.com/update-guide/en-US/vulnerability/CVE-2021-26857)|
|[CVE-2021-26858](https://msrc.microsoft.com/update-guide/en-US/vulnerability/CVE-2021-26858)|
|[CVE-2021-27065](https://msrc.microsoft.com/update-guide/en-US/vulnerability/CVE-2021-27065)|
|[CVE-2021-27078](https://msrc.microsoft.com/update-guide/en-US/vulnerability/CVE-2021-27078)|
 
- [Exchange Team Blog Post - Released: March 2021 Exchange Server Security Updates and patch download links for Exchange 2010, 2013, 2016, 2019 patches](https://techcommunity.microsoft.com/t5/exchange-team-blog/released-march-2021-exchange-server-security-updates/ba-p/2175901)
- [Microsoft Security Response Center release - Multiple Security Updates Released for Exchange Server](https://msrc-blog.microsoft.com/2021/03/02/multiple-security-updates-released-for-exchange-server/)
- [CSS Support: https://support.microsoft.com/](https://support.microsoft.com/)

# Practical notes

## Use CMD instead of Powershell to install updates

**Always install using elevated CMD prompt**.  Ensure that all the Exchange tools are closed on the server (else that can lock up the process).  Avoid using PowerShell due to what it does to path statements.

> Note that the patch takes approximately 30-45 minutes to install.

## Useful Microsoft internal web page to know how to update your servers

- Depending on the Exchange server version (2010, 2013, 2016, 2019) and update level (Roll-Up aka RU for Exchange 2010, CU aka Cumulative Update for Exchange 2013/2016/2019), there are specific steps to get your servers up to date to be able to patch these. The below web site gives you the How-To steps based on your server versions deployed on your environment:

[Exchange How-To patching procedure for all supported Exchange versions]()

## Check Exchange logs to check if you've been compromised

### Scripts to check for possible compromissions

> A script has been released by the Microsoft Support Team ([`Test-ProxyLogon.ps1`](https://github.com/microsoft/CSS-Exchange/tree/main/Security)) that checks if the 4 below breaches have been exploited (CVE-2021-27065, CVE-2021-26857, CVE-2021-26858, CVE-2021-26855)
> 
> Other security related scripts are available on the CSS-Exchange Github page:
> 
> [CSS-Exchange Github page](https://github.com/microsoft/CSS-Exchange/tree/main/Security)

### CVE-2021-26855 - check HTTPProxy logs

- this test is included in the `Test-ProxyLogon.ps1` script mentionned above

- usually located in `%PROGRAMFILES%\Microsoft\Exchange Server\V15\Logging\HttpProxy`

- Use powershell or LogParser to browse and find:

```powershell
Import-Csv -Path (Get-ChildItem -Recurse -Path "$env:PROGRAMFILES\Microsoft\Exchange Server\V15\Logging\HttpProxy" -Filter '*.log').FullName | Where-Object {  $_.AuthenticatedUser -eq '' -and $_.AnchorMailbox -like 'ServerInfo~*/*' } | select DateTime, AnchorMailbox
```

### CVE-2021-26858 - OAB generator log directory

- this test is included in the `Test-ProxyLogon.ps1` script mentionned above

- Directory: %PROGRAMFILES%\Microsoft\Exchange Server\V15\Logging\OABGeneratorLog\*.log

```
findstr /snip /c:"Download failed and temporary file" "%PROGRAMFILES%\Microsoft\Exchange Server\V15\Logging\OABGeneratorLog\*.log"
```

### CVE-2021-26857 - Check application event logs

- this test is included in the `Test-ProxyLogon.ps1` script mentionned above

```powershell
Get-EventLog -LogName Application -Source "MSExchange Unified Messaging" -EntryType Error | Where-Object { $_.Message -like "*System.InvalidCastException*" }
```

### CVE-2021-27065 - ECP log files

- this test is included in the `Test-ProxyLogon.ps1` script mentionned above

- Directory: `C:\Program Files\Microsoft\Exchange Server\V15\Logging\ECP\Server`

```powershell
Select-String -Path "$env:PROGRAMFILES\Microsoft\Exchange Server\V15\Logging\ECP\Server\*.log" -Pattern 'Set-.+VirtualDirectory'
```

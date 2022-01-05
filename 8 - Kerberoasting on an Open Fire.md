## Summary
This challenge opens up after finishing challenge 7 - Printer Exploitation. \
"Obtain the secret sleigh research document from a host on the Elf University domain. What is the first secret ingredient Santa urges each elf and reindeer to consider for a wonderful holiday season? Start by registering as a student on the ElfU Portal (https://register.elfu.org/). Find Eve Snowshoes in Santa's office for hints."

From the challenge name and description, we know this will be a Windows domain and that we'll have to pivot/auth to the various hosts in the domain in order to find the secret sleigh research doc.

Eve Snowshoe will give you a hint after you finish the HoHoNo Fail2Ban challenge in Santa's office. Here's his hint:


## Steps
1. Go to https://register.elfu.org/
2. Register with rocks4socks@elfu.org (found in registration page source code)
It returns access creds:
New Student Domain Account Creation Successful!
You can now access the student network grading system by SSH'ing into this asset using the command below:
ssh iuiqfqxwoj@grades.elfu.org -p 2222
ElfU Domain Username: iuiqfqxwoj
ElfU Domain Password: Cdcmiwbly#

After SSHing in, run ctl d to escape into python, then get a tty interactive shell:

import pty;pty.spawn('/bin/bash')

`nmblookup -A 172.17.0.4`
`smbclient -L \\iuiqfqxwoj -I 172.17.0.4 -N`

`nmap 10.128.3.30 -sV -sC > dc.txt`

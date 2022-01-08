## Summary
Visit Santa's Castle and enter the room to the right and click on the terminal to open the Splunk search page.
- https://hhc21.bossworkshops.io/en-US/account/insecurelogin?username=user&password=kringlecon


[Next Challenge](10%20-%20Now%20Hiring!.md)\
[Back to Table of Contents](https://github.com/minispooner/SANS_KringleCon_2021_Walkthrough/blob/main/README.md)

## Steps
**Task 1**\
Capture the commands Eddie ran most often, starting with git. Looking only at his process launches as reported by Sysmon, record the most common git-related CommandLine that Eddie seemed to use.

<details>
  <summary>Answer</summary>
  git status
</details>

**Task 2**\
Looking through the git commands Eddie ran, determine the remote repository that he configured as the origin for the 'partnerapi' repo. The correct one!

<details>
  <summary>Answer</summary>
  git@github.com:elfnp3/partnerapi.git
</details>

**Task 3**\
Eddie was running Docker on his workstation. Gather the full command line that Eddie used to bring up a the partnerapi project on his workstation.

<details>
  <summary>Answer</summary>
  docker compose up
</details>

**Task 4**\
Eddie had been testing automated static application security testing (SAST) in GitHub. Vulnerability reports have been coming into Splunk in JSON format via GitHub webhooks. Search all the events in the main index in Splunk and use the sourcetype field to locate these reports. Determine the name of the vulnerable GitHub repository that the elves cloned for testing and document it here. Inspect the repository.name field in Splunk.

<details>
  <summary>Answer</summary>
  https://github.com/snoopysecurity
</details>

**Task 5**\
Santa asked Eddie to add a JavaScript library from NPM to the 'partnerapi' project. Determine the name of the library and record it here for our workshop documentation.

<details>
  <summary>Answer</summary>
  holiday-utils-js
</details>

**Task 6**\
Another elf started gathering a baseline of the network activity that Eddie generated. Start with their search and capture the full process_name field of anything that looks suspicious.

Their search link:\
https://hhc21.bossworkshops.io/en-US/app/SA-hhc/search?q=search%20index%3Dmain%20sourcetype%3Djournald%20source%3DJournald%3AMicrosoft-Windows-Sysmon%2FOperational%20EventCode%3D3%20user%3Deddie%20NOT%20dest_ip%20IN%20(127.0.0.*)%20NOT%20dest_port%20IN%20(22%2C53%2C80%2C443)%20%0A%7C%20stats%20count%20by%20dest_ip%20dest_port&display.page.search.mode=smart&dispatch.sample_ratio=1&workload_pool=&earliest=0&latest=now

<details>
  <summary>Answer</summary>
  /usr/bin/nc.openbsd
</details>

**Task 7**\
Uh oh. This documentation exercise just turned into an investigation. Starting with the process identified in the previous task, look for additional suspicious commands launched by the same parent process. One thing to know about these Sysmon events is that Network connection events don't indicate the parent process ID, but Process creation events do! Determine the number of files that were accessed by a related process and record it here.

<details>
  <summary>Answer</summary>
  6
</details>

**Task 8**\
Use Splunk and Sysmon Process creation data to identify the name of the Bash script that accessed sensitive files and (likely) transmitted them to a remote IP address.

<details>
  <summary>Answer</summary>
  preinstall.sh
</details>

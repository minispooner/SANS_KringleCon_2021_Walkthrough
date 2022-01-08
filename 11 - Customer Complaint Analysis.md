## Summary
Download the pcap file from https://downloads.holidayhackchallenge.com/2021/jackfrosttower-network.zip and practice tcpdump or Wireshark searching.

**Objective**: "A human has accessed the Jack Frost Tower network with a non-compliant host. Which three trolls complained about the human? Enter the troll names in alphabetical order separated by spaces. Talk to Tinsel Upatree in the kitchen for hints."

I solved this using tcpdump and did not use the tips from the elf.

[Next Challenge](12%20-%20Frost%20Tower%20Website%20Checkup.md)\
[Back to Table of Contents](https://github.com/minispooner/SANS_KringleCon_2021_Walkthrough/blob/main/README.md)

## Steps
1. To read in the file with tcpdump, run `tcpdump -r jackfrosttower-network.pcap` and you can add `-A` to also show the packet details.
2. Try to find complaints
3. Solve the challenge!

<details>
  <summary>Find Complaints - Answer</summary>
  tcpdump -r jackfrosttower-network.pcap -A | grep "POST /feedback/guest_complaint.php" -A 15
</details>

<details>
  <summary>Find the Trolls - Hint 1</summary>
  Try to find something in common between the three trolls' complaints.
</details>
<details>
  <summary>Find the Trolls - Hint 2</summary>
  "non-compliant" doesn't only refer to TCP packets and RFCs. What about application-level expectations? (above the HTTP stack)
</details>
<details>
  <summary>Find the Trolls - Hint 3</summary>
  Look at the complaint submitter name and trollid. One sticks out.
</details>
<details>
  <summary>Find the Trolls - Answer</summary>
  This command will give you an obvious fishy smell and should lead you to the answer:
  
  tcpdump -r jackfrosttower-network.pcap -A | grep "POST /feedback/guest_complaint.php" -A 15 | grep name | cut -d"&" -f2 | sed 's/+/ /g'
  
  The complainer's room number is found in 3 trolls' complaints:
  
  - tcpdump -r jackfrosttower-network.pcap -A | grep "POST /feedback/guest_complaint.php" -A 15 | grep name | grep 1024 | cut -d"&" -f1
  
  Answer: "Flud Hagg Yaqh"
</details>

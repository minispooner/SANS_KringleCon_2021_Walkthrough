## Summary
Enter Santa's Castle and go to the Talks floor. Click the IPv6 Sandbox terminal to enter the challenge and practice network enumeration!

## Steps
1. Discover UP hosts on the network
<details>
  <summary>Hint</summary>
  nmap has a no port scan flag (-sn) and you can scan CIDR ranges, like x.x.x.0/24
</details>
2. Discover open ports and services on the UP hosts
<details>
  <summary>Hint</summary>
  nmap 192.168.160.2
</details>
3. Perform recon on the UP hosts
<details>
  <summary>Hint 1</summary>
  curl http://192.168.160.2
</details>
<details>
  <summary>Hint 2</summary>
  curl http://ipv6-server.ipv6guest.kringlecastle.com
</details>
4. Perform more recon on open ports
<details>
  <summary>Hint 1</summary>
  nmap -6 2604:...:e405
</details>
5. Get the answer!
<details>
  <summary>Hint</summary>
  curl http://[2604:...:e405]:9000
</details>
<details>
  <summary>Answer</summary>
  PieceOnEarth
</details>

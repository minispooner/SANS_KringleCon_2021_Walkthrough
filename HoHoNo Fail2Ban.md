## Summary
Enter Santa's office to complete the HoHoNo Fail2Ban challenge, which is a prerequisite to obtain Eve Snowshoes' hint for the Kerberoasting challenge.

## Steps
1. Watch https://www.youtube.com/watch?v=Fwv2-uV6e5I


Example log lines:
```
2022-01-05 04:39:03 Valid heartbeat from 103.247.53.95
2022-01-05 04:39:04 167.107.246.57: Request completed successfully
2022-01-05 04:39:05 Login from 157.100.91.51 successful
2022-01-05 04:39:05 Login from 22.186.67.151 successful
2022-01-05 09:21:25 Failed login from 163.49.35.129 for jewel
2022-01-05 09:22:32 Failed login from 163.49.35.129 for prancer
2022-01-05 09:22:36 Failed login from 22.231.17.27 for krampus
2022-01-05 04:39:06 Valid heartbeat from 166.20.237.56
2022-01-05 04:39:07 90.180.152.111: Request completed successfully
2022-01-05 05:44:48 Login from 216.44.122.175 rejected due to unknown user name
2022-01-05 05:47:43 216.44.122.175 sent a malformed request
2022-01-05 05:46:32 Invalid heartbeat 'delta' from 170.137.135.68
```

vim /etc/fail2ban/filter.d/ms.conf
```
[Definition]
failregex = ^[\d\-: ]+Failed login from <HOST> for .+$
            ^ <HOST> sent a malformed request$
            ^[\d\-: ]+Login from <HOST> rejected due to unknown user name$
            ^[\d\-: ]+Invalid heartbeat '[\w]+' from <HOST>$
```

vim /etc/fail2ban/action.d/ms.conf
```
[Definition]
actionstart = echo "starting"
actionstop = echo "stopping or refresh?"
actioncheck = /root/naughtylist list
actionban = /root/naughtylist add <ip>
actionunban = /root/naughtylist del <ip>
```

vim /etc/fail2ban/jail.d/ms.conf
```
[ms]
enabled = true
logpath = /var/log/hohono.log
findtime = 60m
maxretry = 10
bantime = 60m
filter = ms
action = ms
```

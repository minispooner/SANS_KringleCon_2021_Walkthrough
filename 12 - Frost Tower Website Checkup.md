## Summary
At the top right of the main courtyard, after thawing the Frost Tower door and fixing the Frostivator elevator, go to the top floor, Jack's Office. Enter the left room, called Studio, and talk with Ingreta about the Frost website and download the sourcecode. You will practice sourcecode review and web application hacking.

## Steps
1. Review the sourcecode for flaws
- If you aren't able to find anything, you can go complete the Python coding challenges (where you move the little elf to collect candies), and after level 8, the elf there will give you a few hints about where the Frost Tower Website is vulnerable.
<details>
  <summary>The Elf's Hint</summary>
  He tells you to review a couple of lib docs - the npm express-session package (https://www.npmjs.com/package/express-session) and the mysqljs/mysql package (https://github.com/mysqljs/mysql).
</details>

2. In the sourcecode...(base64 decode me!)
<details>
  <summary>Hint 1</summary>
  The server.js file imports the express-session lib. Take a look at how it uses this lib to find developer flaws in session management.
</details>

<details>
  <summary>Hint 2</summary>
  The server.js file assigns the unauthenticated user a session when a certain unauthenticated call is made with certain logic. Try to trigger it and verify a valid session by browsing to the session-protected /dashboard page.
</details>

<details>
  <summary>Hint 3</summary>
  The server.js /postcontact method assigns the user a session when certain logic is followed. Try to trigger it and verify a valid session by browsing to the session-protected /dashboard page.
</details>

<details>
  <summary>Answer 1</summary>
  Submit the /contact form using some email address. Submit the form again, using the same email address. You now have a session and can navigate to /dashboard to confirm.
</details>

## Summary
This is the sleigh challenge on the top of Santa's Castle - the NetWars roof. The elf tells you that you're supposed to modify 2 client-side values, one of which is passed to the backend, to enable 2-player mode and refuel Santa's tank. Then he'll give you a tip for the Printer Exploitation objective.

## Steps
Knowing the instructions, try to find 1 local variable (not passed to the server) that stands out that you could change, and find another variable that IS passed to the server that you can change. The result will be that the Computer player2 joins and plays along with you.

<details>
  <summary>Server Var - Hint 1</summary>
  Inspect your network traffic to find interesting variables that could indicate the addition of another player.
</details>

<details>
  <summary>Server Var - Hint 2</summary>
  HOHOHO
</details>

<details>
  <summary>Local Var - Hint 1</summary>
  Where could you find local variables that are not passed to the server? What run's or is found locally? (not HTTP params or headers or part of any web request).
</details>

<details>
  <summary>Local Var - Hint 2</summary>
  JavaScript stores local variables.
</details>

<details>
  <summary>Local Var - Hint 3</summary>
  Take a look at the top of holidayhero.min.js and look for interesting variables that you can change locally. You can change variables direcly in your browser developer tools. Be sure to click into the context of your game's iframe.
</details>

<details>
  <summary>Local Var - Hint 4</summary>
  single_player_mode
</details>

<details>
  <summary>Answer</summary>
  In browser developer console, after joining a random table:
  
1. Change the storage Cookie value of singleplayer to equal true: HOHOHO: %7B%22single_player%22%3Atrue%7D
  
2. Run this javascript in the developer console: single_player_mode=true
</details>

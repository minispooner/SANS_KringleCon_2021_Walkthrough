## Summary
Inside the Jack Frost tower, click a slot machine and practice web api hacking!

[Back to Table of Contents](https://github.com/minispooner/SANS_KringleCon_2021_Walkthrough/blob/main/README.md)

## Steps
1. Open up BurpSuite and scope the target to the web app or start persisting network traffic in your browser's developer console. Get to know the few main web app calls that your client makes.
2. Try to get 1000+ credits!

<details>
  <summary>Hint 1</summary>
  After finding the /spin api call, I tried modifying the betamount param with N>credit_amount (like 1001) but that failed. I then tried raising the cpl field to a large ammount and also tried changing the numline but both failed as well. At this point, how could we beat the odds? Try scripting it!
</details>

<details>
  <summary>Answer</summary>

```
import requests

for i in range(0,10):
    s = requests.Session()
    s.get('https://slots.jackfrosttower.com/')
    # print(r.cookies['slots_session'])
    r = s.get('https://slots.jackfrosttower.com/api/v1/02b05459-0d09-4881-8811-9a2a7e28fd45/session')
    session_id = r.json()['data']['session_id']

    for j in range(0,10):
        h = {'X-Ncash-Token': session_id}
        r = s.post(
            'https://slots.jackfrosttower.com/api/v1/02b05459-0d09-4881-8811-9a2a7e28fd45/spin',
            data={
                'betamount':'10',
                'numline':20,
                'cpl':0.5
            },
            headers=h
        )
        if not r.json()['success'] or r.json()['data']['credit']<10:
            break
        else:
            print(r.text)
```
  
  Answer text: "I'm going to have some bouncer trolls bounce you right out of this casino!"
</details>


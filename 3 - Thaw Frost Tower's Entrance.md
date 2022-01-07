## Summary
Towards the top right of the main courtyard, talk to Grimy McTrollkins to learn about thawing the Frost Tower entrance. You will practice WiFi enumeration and API curling.

[Back to Table of Contents](https://github.com/minispooner/SANS_KringleCon_2021_Walkthrough/blob/main/README.md)

## Steps
1. Use the WiFi adapter you picked up in the KringleCon Orientation (Click Hammer at left of game screen)
2. Open the WiFi CLI
3. Use the two commands to find the WiFi channel and connect to it
- `iwlist wlan0 scan` - will return the WiFi name "FROST-Nidus-Setup"
- `iwconfig wlan0 mode Managed essid 'FROST-Nidus-Setup'` (no key needed) - will connect you to the thermostat's WiFi network and give you access to the thermostat's HTTP API at http://nidus-setup:8080/
4. Enumerate the API info and increse the tempurature
- `curl http://nidus-setup:8080/` - gives you the /apidoc resource
- `curl http://nidus-setup:8080/apidoc` - gives you the /api/cooler endpoint and an example POST request format
- `curl http://nidus-setup:8080/api/cooler -X POST -H 'Content-Type: application/json' --data-binary '{"temperature": 100}'` - returns "ICE MELT DETECTED!" warning, thus melting the tower door.

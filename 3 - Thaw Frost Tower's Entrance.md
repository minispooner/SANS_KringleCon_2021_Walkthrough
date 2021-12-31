## Summary
Towards the top right of the main courtyard, talk to Grimy McTrollkins to learn about thawing the Frost Tower entrance. You will practice WiFi enumeration and API curling.

## Steps
1. Use the WiFi adapter you picked up in the KringleCon Orientation (Click Hammer at left of game screen)
2. Open the WiFi CLI
3. Use the two commands to find the WiFi channel and connect to it
- `iwlist wlan0 scan` - will return the WiFi name (this is what I did the first time but it didn't work when I reregistered to verify the command)
- `iwconfig wlan0 mode Managed essid 'my_network'` (no key needed) - will "connect" you to the WiFi network (haven't verified since I reregistered). Just Google how to connect using iwconfig.

After obtaining access to the network, you are able to see the "internal" web API. The next steps use curl to hit the HTTP API. I think I curled the root path, then /docs, then it shows which API paths are available WITHOUT REGISTRATION. It will also give you an example curl POST that you can copy and modify and run in order to raise the temperature to 100 degrees and thaw the door.

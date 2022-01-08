## Summary
Enter Santa's Castle, go to the Talks floor, enter the left room and talk to the elf and open the Strange USB Device terminal. Practice Rubber Ducky scripting language!

[Next Challenge](6%20-%20Shellcode%20Primer.md)\
[Back to Table of Contents](https://github.com/minispooner/SANS_KringleCon_2021_Walkthrough/blob/main/README.md)

## Steps
1. Figure out how to decode the inject.bin file!
<details>
  <summary>Hint 1</summary>
  Try Googling for open source Duck Script decoders. Be sure to narrow your search based on available bins on the system (no Java for example)
</details>


<details>
  <summary>Hint 2</summary>
  You have to build your own Duck Script parser with the available python bin. I created a copy/paste-able script by extracting code from https://github.com/kevthehermit/DuckToolkit/blob/master/ducktoolkit/decoder.py
</details>

<details>
  <summary>Answer</summary>
  Run the below in a python session, then extract the base64 output command at the end of the results, exit() from python session, and b64 | rev | decode it!
  
  Answer = "ickymcgoop"
  
```
from binascii import hexlify
ducky_bin=open('/mnt/USBDEVICE/inject.bin', 'rb').read()
ducky_hex=hexlify(ducky_bin)
decoded_bin = ""
duck_decoded = ""
major_version = 3

lang_file={
"__comment":"All numbers here are in hex format and 0x is ignored.",
"__comment":" ",
"__comment":"This list is in ascending order of 3rd byte (HID Usage ID).",
"__comment":"  See section 10 Keyboard/Keypad Page (0x07)",
"__comment":"  of document USB HID Usage Tables Version 1.12.",
"__comment":" ",
"__comment":"Definition of these 3 bytes can be found",
"__comment":"  in section B.1 Protocol 1 (Keyboard)",
"__comment":"  of document Device Class Definition for HID Version 1.11",
"__comment":"  - byte 1: Modifier keys",
"__comment":"  - byte 2: Reserved",
"__comment":"  - byte 3: Keycode 1",
"__comment":" ",
"__comment":"Both documents can be obtained from link here",
"__comment":"  http://www.usb.org/developers/hidpage/",
"__comment":" ",
"__comment":"A = LeftShift + a, { = LeftShift + [",
"__comment":" ",
"a":"00,00,04",
"b":"00,00,05",
"c":"00,00,06",
"d":"00,00,07",
"e":"00,00,08",
"f":"00,00,09",
"g":"00,00,0a",
"h":"00,00,0b",
"i":"00,00,0c",
"j":"00,00,0d",
"k":"00,00,0e",
"l":"00,00,0f",
"m":"00,00,10",
"n":"00,00,11",
"o":"00,00,12",
"p":"00,00,13",
"q":"00,00,14",
"r":"00,00,15",
"s":"00,00,16",
"t":"00,00,17",
"u":"00,00,18",
"v":"00,00,19",
"w":"00,00,1a",
"x":"00,00,1b",
"y":"00,00,1c",
"z":"00,00,1d",
"1":"00,00,1e",
"2":"00,00,1f",
"3":"00,00,20",
"4":"00,00,21",
"5":"00,00,22",
"6":"00,00,23",
"7":"00,00,24",
"8":"00,00,25",
"9":"00,00,26",
"0":"00,00,27",
"ENTER":"00,00,28",
"ESC":"00,00,29",
"ESCAPE":"00,00,29",
"TAB":"00,00,2b",
" ":"00,00,2c",
"SPACE":"00,00,2c",
"-":"00,00,2d",
"=":"00,00,2e",
"[":"00,00,2f",
"]":"00,00,30",
"\\":"00,00,31",
";":"00,00,33",
"'":"00,00,34",
"`":"00,00,35",
",":"00,00,36",
".":"00,00,37",
"/":"00,00,38",
"CAPSLOCK":"00,00,39",
"F1":"00,00,3a",
"F2":"00,00,3b",
"F3":"00,00,3c",
"F4":"00,00,3d",
"F5":"00,00,3e",
"F6":"00,00,3f",
"F7":"00,00,40",
"F8":"00,00,41",
"F9":"00,00,42",
"F10":"00,00,43",
"F11":"00,00,44",
"F12":"00,00,45",
"PRINTSCREEN":"00,00,46",
"SCROLLLOCK":"00,00,47",
"BREAK":"00,00,48",
"PAUSE":"00,00,48",
"INSERT":"00,00,49",
"HOME":"00,00,4a",
"PAGEUP":"00,00,4b",
"DEL":"00,00,4c",
"DELETE":"00,00,4c",
"END":"00,00,4d",
"PAGEDOWN":"00,00,4e",
"RIGHT":"00,00,4f",
"RIGHTARROW":"00,00,4f",
"LEFT":"00,00,50",
"LEFTARROW":"00,00,50",
"DOWN":"00,00,51",
"DOWNARROW":"00,00,51",
"UP":"00,00,52",
"UPARROW":"00,00,52",
"APP":"00,00,65",
"MENU":"00,00,65",
"ALT-TAB":"00,00,71",
"CONTROL":"01,00,00",
"CTRL":"01,00,00",
"SHIFT":"02,00,00",
"A":"02,00,04",
"B":"02,00,05",
"C":"02,00,06",
"D":"02,00,07",
"E":"02,00,08",
"F":"02,00,09",
"G":"02,00,0a",
"H":"02,00,0b",
"I":"02,00,0c",
"J":"02,00,0d",
"K":"02,00,0e",
"L":"02,00,0f",
"M":"02,00,10",
"N":"02,00,11",
"O":"02,00,12",
"P":"02,00,13",
"Q":"02,00,14",
"R":"02,00,15",
"S":"02,00,16",
"T":"02,00,17",
"U":"02,00,18",
"V":"02,00,19",
"W":"02,00,1a",
"X":"02,00,1b",
"Y":"02,00,1c",
"Z":"02,00,1d",
"!":"02,00,1e",
"@":"02,00,1f",
"#":"02,00,20",
"$":"02,00,21",
"%":"02,00,22",
"^":"02,00,23",
"&":"02,00,24",
"*":"02,00,25",
"(":"02,00,26",
")":"02,00,27",
"_":"02,00,2d",
"+":"02,00,2e",
"{":"02,00,2f",
"}":"02,00,30",
"|":"02,00,31",
":":"02,00,33",
"\"":"02,00,34",
"~":"02,00,35",
"<":"02,00,36",
">":"02,00,37",
"?":"02,00,38",
"CTRL-SHIFT":"03,00,00",
"ALT":"04,00,00",
"CTRL-ALT":"05,00,00",
"ALT-SHIFT":"06,00,00",
"COMMAND":"08,00,00",
"GUI":"08,00,00",
"WINDOWS":"08,00,00",
"COMMAND-OPTION":"12,00,00",
"COMMAND-CTRL-SHIFT":"12,00,00",
"COMMAND-CTRL":"12,00,00",
"COMMAND-OPTION-SHIFT'":"12,00,00"
}

decoder_command_keys = [
"DELAY",
"SPACE",
"CTRL",
"ALT",
"GUI",
"WINDOWS",
"ESC",
"ESCAPE",
"PRINTSCREEN",
"INSERT",
"HOME",
"DELETE",
"END",
"ENTER",
"PAGEUP",
"PAGEDOWN",
"LEFTARROW",
"LEFT",
"DOWNARROW",
"DOWN",
"RIGHTARROW",
"RIGHT",
"UPARROW",
"UP",
"SCROLLLOCK",
"WINDOWS",
"MENU",
"TAB",
"CAPSLOCK",
"F1",
"F2",
"F3",
"F4",
"F5",
"F6",
"F7",
"F8",
"F9",
"F10",
"F11",
"F12",
"GUI R",
"GUI D",
"CTRL-ALT",
"CTRL-SHIFT",
"ALT-SHIFT"
"CONTROL",
"ESCAPE",
"DELAY",
"DEFAULTDELAY",
"DEFAULT_DELAY",
"CTRL S",
"CTRL V",
"CTRL X",
"CTRL Z",
"CTRL C",
"ALT F4",
"WAKE",
"SLEEP",
"APP",
"STOP",
"POWER"
]

for i in range(0, len(ducky_hex), 4):
    decoded_key = ""
    last_key = duck_decoded
    duck_decoded = ducky_hex[i:i+4]
    for key, value in lang_file.items():
        # Convert value from new format
        try:
            new_value = value.split(',')
            if len(new_value) == 3:
                value = '{0}{1}'.format(new_value[2], new_value[0])
                if major_version == 3:
                    value = bytes(value, 'utf-8')
        except Exception as e:
            continue
        # Fix for spacing in STRING statements
        if duck_decoded == b"2c00":
            decoded_key = " "
        elif duck_decoded == b"00ff" and last_key !=b"00ff":
            decoded_key = "DELAY"
        elif duck_decoded == value:
            decoded_key = key
        else:
            # print(duck_decoded)
            if duck_decoded[-2:] == b"00":
                if duck_decoded[:2] == value:
                    if len(key) == 1:
                        decoded_key = key
    if decoded_key in decoder_command_keys:
        decoded_bin += decoded_key + "\n"
    else:
        decoded_bin += decoded_key

print('########################')
print('########################')
print('########################')
print('########################')
print(decoded_bin)
print('DONE')
```
  
</details>

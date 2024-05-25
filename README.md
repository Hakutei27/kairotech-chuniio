
# KairoTech ChuniIO

Updated on 2024/5/26

## Features
- Basic Features
- Supports hot swapping and will not cause game timeout
## How To Use
### For Customers:
Add the following text to segatools.ini

[chuniio]  
path=KairoTech.dll

### For Producter

- Please be sure that your VID is 0x303A and your PID is 0x81F7.
- If your HID device has only one report, the LED function will be disabled, please set all led's color by yourself.

#### Communication
- This library use raw USB HID for communicating, datas has 2 section, the first section is datas from controller to game, the communication content is like this:

    01 00 00 00 00 00 00 00  
    00 00 00 00 00 00 00 00  
    00 00 00 00 00 00 00 00  
    00 00 00 00 00 00 00 00  
    00 00
    
    The first byte is reportID, this byte is always be 0x01, you don't need to change this.The following 32 bytes are touch value, from 入力１(touch1) to 入力３２(touch32).The last byte is a union of 6 air-strings and test, service button, the 1st-6th bits are 6 air-strings, the 7th bit is test and the 8th bit is service.

- The second section is datas from game to controller, the communication content is like this:

    02 00 00 00 00 00 00 00  
    00 00 00 00 00 00 00 00  
    00 00 00 00 00 00 00 00  
    00 00 00 00 00 00 00 00  
    00 00 00 00 00 00 00 00  
    00 00 00 00 00 00 00 00  
    00
    
    The first byte is reportID, this byte is always be 0x02, you don't need to change this.The following 48 bytes is the slider's led data, per led has 3 bytes of data contains R, G and B, you need to notice that this data pack has no led data of splits,led data of splits has another data pack like this: 

    03 00 00 00 00 00 00 00  
    00 00 00 00 00 00 00 00  
    00 00 00 00 00 00 00 00  
    00 00 00 00 00 00 00 00  
    00 00 00 00 00 00 00 00  
    00 00 00 00 00 00 00 00  
    00

    It's same with data pack 02


# KairoTech ChuniIO

Updated on 2024/7/12

## Features
- Basic Features
- Supports hot-plug and won't cause game timeout
## For Customers:
Add the following text to segatools.ini

[chuniio]  
path=KairoTech.dll

## For Producter

- Please be sure that your Vendor ID is `0x303A` and your Product ID is `0x81F7`.
- If your HID device has only one report, the LED function will be disabled.

## Communication
- This library use raw USB HID for communicating.

### Controller -> Game

- Raw HID Data
```
    01 00 00 00 00 00 00 00  
    00 00 00 00 00 00 00 00  
    00 00 00 00 00 00 00 00  
    00 00 00 00 00 00 00 00  
    00 00
```
- Struct

```
struct
{
    unsigned char reportID;
    uint8_t touchValue[32];
    uint8_t ir;
    uint8_t state;
};
```
    
- `reportID` is 0x01, don't change this.
- `touchValue` are touch values from 入力１(touch1) to 入力３２(touch32).
- `ir` is 6 air-string's data, only 6 bit are used.
- `state` is test, service and coin button, only 3 bit are used.

### Game -> Controller
- Raw HID Data
```
    02 00 00 00 00 00 00 00  
    00 00 00 00 00 00 00 00  
    00 00 00 00 00 00 00 00  
    00 00 00 00 00 00 00 00  
    00 00 00 00 00 00 00 00  
    00 00 00 00 00 00 00 00  
    00
```
- Struct
```
struct
{
    unsigned char reportID;
    unsigned char packageID;
    union
    {
        uint8_t sliderled[48];      // packageID 0x00, touch led
        uint8_t splitled[45];       // packageID 0x01, split led
        uint8_t airled[18];         // packageID 0x02, air led
        uint8_t billboardled[60];   // packageID 0x03-0x08, billboard led
    };
};
```

- `reportID` is 0x02, don't change this.
- LED data has 3 channel, so 1 led data need 3 byte, like 'r g b r g b r g b'.
- LED data will NOT transfer all the time, it only transfer when led data changed.For example, when air led data changed, a package that packageID is 0x02 will be transferd.

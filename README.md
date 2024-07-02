
# KairoTech ChuniIO

Updated on 2024/7/2

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
#### Package 1 (Touch Area Light Package)
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
    uint8_t rgb[48];
};
```

- `reportID` is 0x02, don't change this.
- `rgb` are r,g,b values for 16 lights.

#### Package 2 (Split Area Light Package)

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
    uint8_t rgb[45];
};
```

- `reportID` is 0x03, don't change this.
- `rgb` are r,g,b values for 15 lights (split between 2 touch area).

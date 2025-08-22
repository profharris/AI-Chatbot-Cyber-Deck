# AI Chatbot Cyber Deck
https://docs.keyestudio.com/projects/ESP32S3_LCD154/en/latest/LCD154/LCD154.html<br/>
https://github.com/78/xiaozhi-esp32

### Design follows inspirations from:
 1. ESP32-S3 1.54in TFT Expansion Board with Speaker
 2. XIAOZHI AI Voice Chatbox ESP32-S3-N16R8
 3. Xiao Zhi AI Chatbot Breadboard DIY Kit

### On-board Peripherals:
 1. SPI (software) ***ST7789 1.54” 240×240 TFT Display:***
 2. SPI (hardware) ***SPI microSD_Card Reader/Writer:***
 3. I²S ***INMP441 Microphone:***
 4. I²S ***MAX98357A Amplifier/Speaker:***
 5. I²C ***Stemma-Qt/Qwiic 2-port I²C interface:***
 6. Buttons [Vol_UP ⏶], [OK ⏴], [Volume ⏷]
 7. WS2812 NeoPixel ***RGB LED*** (RGB_BUILTIN)

### MCU Development Board (ESP32-S3-WROOM-1):

#### Espressif ESP-S3-DevKitC-1 — &nbsp;44-pins, 22-pin Headers
```
ESP32-S3-WROOM-1 “pin-compliant” Development boards:     Pins   RGB LEDs
                                                                LED
 1. Espressif ESP32-S3-DevKitC-1 (no camera, no SD card)  44     ✓  1+RGB
 2. YD-ESP32-S3 (ESP32-S3-WROOM-1 Dev) (DevKitC-1 clone)  44     ✓  3+RGB
 3. WeAct Studio ESP32-S3-A/B Core     (DevKitC-1 clone)  44     ✓  1+RGB
 4. WaveShare ESP32-S3-DEV-KIT-N8R8    (DevKitC-1 clone)  44     ✓  1+RGB

 5. FREENOVE ESP32-S3-WROOM-Lite (no camera)              40     ✓  4+RGB
    (40-pin boards plug in skipping the top & bottom rows of pins.)
```
> [!IMPORTANT]
> [1. Espressif ESP32-S3-DevKitC-1](https://github.com/profharris/ESP32-S3-1.54in-TFT-Expansion-Board-with-Speaker/blob/main/images/1.%20ESP32-S3-DevKitC-1%20(44-pins).jpg)<br/>
> [2. YD-ESP32-S3 (ESP32-S3-WROOM-1 Dev)](https://github.com/profharris/ESP32-S3-1.54in-TFT-Expansion-Board-with-Speaker/blob/main/images/2.%20YD-ESP32-S3%20(ESP32-S3-DevKitC-1%20clone)%20(44-pins).jpg)<br/>
> [3. WeAct Studio ESP32-S3-A/B Core](https://github.com/profharris/ESP32-S3-1.54in-TFT-Expansion-Board-with-Speaker/blob/main/images/3.%20WeAct%20Studio%20ESP32-S3-AB%20Core%20(44-pins).jpg)<br/>
> [4. WaveShare ESP32-S3-DEV-KIT-N8R8](https://github.com/profharris/ESP32-S3-1.54in-TFT-Expansion-Board-with-Speaker/blob/main/images/4.%20WaveShare%20ESP32-S3-DEV-Kit-N8R8%20(44-pins).jpg)<br/>
><br/>
> [5. FREENOVE ESP32-S3-WROOM-Lite](https://github.com/profharris/ESP32-S3-1.54in-TFT-Expansion-Board-with-Speaker/blob/main/images/8.%20FREENOVE%20ESP32-S3-WROOM-Lite%20(40-pins).jpg)
<hr><br/>


## 【1】 MCU ESP32-S3-WROOM-1 Design Guide:
```
————————————————————————————————————————————————————————————————————    ————————————
YD-ESP32-S3 (ESP32-S3-WROOM-1 Dev)  (44-Pin)    ESP32-S3 Dev Module       I²C QWIIC
                                                                        ————————————
Xtensa® 32-bit         ESP32-S3-DevKit-C-1  ESP32-S3-WROOM-1 N16R8      1   *   GND
dual-core LX7               YD-ESP32-S3                                 2   *   3V3
240MHz,   512KB SRAM      _______________   NO CAMERA MODULE            3 GPIO8 SDA
16MB ƒlash, 8MB PSRAM    |  ___   _   __¯|  NO SD-CARD                  4 GPIO9 SCL
WiFi 802.11 b/g/n        | | | |_| |_|   |  GPIO_ = Strapping Pins
BLE®5                 .——| ' '           |——.                           ————————————
               3V3   1|o:|ESP32S3-WROOM-1|:o|44 ⏚GND                    Hardware SPI
               3V3   2|o:|               |:¤|43 IO43 [U0TXD›  ҉LED]     microSD_Card
[RESET/RST   ] EN    3|o:| .··. . F© Œ Æ |:¤|42 IO44 [U0RXD‹  ҉LED]     ————————————
[I²S Mic WS  ] IO4   4|o:| WiFi ß   ____ |:◌|41 IO1  [A0          ]     GPIO10 CS
[I²S Mic SCK ] IO5   5|o:|         |QRCD||:◌|40 IO2  [A1          ]     GPIO11 MOSI
[I²S Mic SD  ] IO6   6|o:| ° N16R8 |____||:o|39 IO42 [SPI TFT_BL  ]     GPIO12 SCK
[I²S Amp DIN ] IO7   7|o:'———————————————':o|38 IO41 [SPI TFT_CS  ]     GPIO13 MISO
[I²S Amp BCLK] IO15  8|o:                 :o|37 IO40 [SPI TFT_DC  ]
[I²S Amp LRC ] IO16  9|o  :: ‡‡‡    · RST  o|36 IO39 [   Vol_UP⏶ ]     ————————————
[A16         ] IO17 10|◌  ¨¨|¯¯¯¬   : [◙]  o|35 IO38 [   Vol_DN⏷ ]     SPI 1.54in
[A17         ] IO18 11|◌  ¨¨|LDO[]  : BOOT ●|34 IO37 [OPI PSRAM  ●]     240×240 TFT
[A7  I²C SDA ] IO8  12|o  ¨¨|___- ¬¬  [◙]  ●|33 IO36 [OPI PSRAM  ●]     ————————————
[A2          ] IO3_ 13|◌  ·  ‡‡‡  ¨¨       ●|32 IO35 [OPI PSRAM  ●]     GPIO42 BL
[IN ONLY     ] IO46 14|◌            P T R  o|31 IO0_ [BOOT ⏴OK⏵ ]     GPIO41 CS
[A8  I²C SCL ] IO9  15|o  RGB RGB   R X X  o|30 IO45_[SPI TFT_RST ]     GPIO40 DC
[A9  SPI_CS  ] IO10 16|o  [҉]  ⯼    ҉ ҉ ҉  ¤⏐29 IO48 [RGB ҉WS2812҉]     GPIO45 RST
[A10 SPI_MOSI] IO11 17|o           ··· ___ o|28 IO47 [SPI TFT_MOSI]     GPIO47 MOSI
[A11 SPI_SCK ] IO12 18|oIN-OUT ‡‡‡ :::|343|o|27 IO21 [SPI TFT_SCLK]     GPIO21 SCLK
[A12 SPI_MISO] IO13 19|o ⯼            |___|ø⏐26 IO20 [A19   USB_D⧾]
[A13         ] IO14 20|◌  _____ O T _____  ø|25 IO19 [A18   USB_D⧿]     ————————————
[      IN-OUT]  5V0 21|o | USB |T T| USB | o|24 ⏚GND                    INMP441 Mic
                GND 22|o |  C  |G L|  C  | o|23 ⏚GND                    ————————————
                      '——'ESP32'———'UART0'——'                           GPIO4  WS
                                                IO48 RGB_BUILTIN,       GPIO5  SCK
🟥Red PWR LED, 🟩Green TX LED, 🟦Blue RX LED        LED_BUILTIN        GPIO6  SD

 1. I²S INMP441  Microphone             4. SPI 1.54in 240×240 TFT       ————————————
 2. I²S MAX98357A Audio Amp/Speaker     5. SPI microSD_Card             I²S MAX98357
 3. I²C Qwiic/Stemma-Qt interface       6. [Push Buttons] ◙             Amp/Speaker
                                                                        ————————————
ESP32-S3 Pins: 0…18 GPIO, 19…20 D+/D-, 21 GPIO, 22…25 Do Not Exist,     GPIO7  DIN
26…32 QSPI ƒlash, 33…34 N/A, 35…42 GPIO, 43…44 TX/RX, 45…48 GPIO.       GPIO15 BCLK
 pins_arduino.h ~ ESP32-S3-DevKitC-1                                    GPIO16 LRC
————————————————————————————————————————————————————————————————————    ————————————
```
<br/><hr>

## 【2】 Vero-Stripboard Layout and Design:

Based on a Vero-Stripboard,&nbsp; all horizontal connections are the copper
traces of the Veroboard _(with two small exceptions)_.&nbsp; All vertical
connections are only _straight wires_ on the top of the Veroboard,&nbsp;
soldered on the back.&nbsp; Obviously,&nbsp; to prevent cross-wires & shorts,
&nbsp;‘cuts’ must be made to the horizontal Veroboard copper traces as
denoted by &nbsp;“X”&nbsp; in the diagrams delow.

The majority of the working GPIO Pins (not counting 3V3 and GND) for the
peripheral devices will NOT need Vertical jumpers, due to clever alignment
of the peripherals Pin interfaces with the **ESP32-S3-WROOM-1** Development
Boards and the horizontal copper traces of the Veroboard base. The exception
to this is the 3×2 pin INMP441 I²S Microphone, which admittedly is simply a
best fix placement.

The two horizontal connection exceptions,&nbsp; made on the bottom of the
Veroboard,&nbsp; are for the _Button pins_ GPIO38 and GPIO39 to connect
around ‘cuts’ made to the Veroboard,&nbsp; for the _LCD 1.54in 240RGB×240_
8-pin header.&nbsp; These Horzontal connect wires are denoted by ‘»’ and
‘«’ in the Veroboard Right-side diagram below.<br>
¦µ×÷±¡¿¸‚„…ƒ¬–—·•‹›«»¨¯´ˆ˜‘’“”¹²³ª°®™©¢çÇÒÓÔÕÖØÞ⩇ðòóôõöø¤§†‡ÎÏßíìîïÐ

The 6.5×14.5cm Vero-Stripboard is 56 _2.54mm spaced holes_ horizontally
and 24 _2.54mm spaced holes_ vertically.&nbsp; The 44-pin **ESP32-S3-WROOM-1**
based Development Boards _(ESP32-S3-DevKitC-1 compatible)_ are 22 pins tall.&nbsp;
These Development Boards are mounted with their lower pins lined up with
the last/lowest row on the Veroboard,&nbsp; leaving the top two rows open.

The Right-side IPX female headers for the **ESP32-S3-WROOM-1** Development
Board are “double-wide”.&nbsp; The Development Board plugs into the _inner/left_
header,&nbsp; leaving the _outside/right_ header exposed for access to the
Development Board GPIO pins.

The Left-side IPX female headers for the **ESP32-S3-WROOM-1** Development
Boards are “tripple-wide”.&nbsp; This is because the original Expressif
**ESP32-S3-DevKitC-1** is 1 pin-space narrower than most availiable
clone Development boards,&nbsp; and so will plug into the _inner/right_ of
the Left-side headers. Most **ESP32-S3-WROOM-1** based Development Boards
will plug into the _center_, Left-side header,&nbsp; leaving the _outside/left_
header exposed for access to the Development Board GPIO pins.

### Hardware Components:
  1. YD-ESP32-S3 (ESP32-S3-WROOM-1 Dev) -or compatible
  2. SPI LCD 1.54in 240RGB×240 IPS TFT (ST7789)
  3. SPI microSD_Card Reader/Writer
  4. I²S INMP441 MEMS Mic
  5. I²S MAX98357A Amp/Speaker
  6. 3W 4Ω Speaker
  7. I²C Qwiic/Stemma-Qt dual-port interface
  8. I²C Qwiic/Stemma-Qt sensors as desired

### Build Components:
  1. 1× Stripboard Veroboard (6.5×14.5cm) 24×56 2.54mm holes
  2. 2×  3-pin IPX Male headers Black  ⏚GND headers
  3. 1×  3-pin IPX Male headers Red    5V0 headers
  4. 1×  3-pin IPX Male headers Orange 3V3 Headers
  5. 2×  3-pin IPX Female headers      INMP441 I²S Mic
  6. 1×  7-pin IPX Female header       MAX98357A I²S Amp
  7. 1×  6-pin IPX Female header       microSD_Card SPI Read/Write
  8. 1×  4-pin IPX Female header       I²C Qwiic/Stemma-QT interface
  9. 1×  8-pin IPX Female header       LCD 1.54in SPI 240RGB×240
 10. 5× 22-pin IPX Female headers      YD-ESP32-S3 (ESP32-S3-WROOM-1)
 11. 4× Buttons (tactile switches)     Vol_UP, Btn_OK, Vol_DN, Reset
 12. 1× Battery Power On/Off switch

### Vertical Header sockets: (11)

 | Hardware Component         | Number of Pins   |    Location     |
 |:---------------------------|:-----------------|:---------------:|
 | 1. I²S MAX98357A Amp/SPKR  |    7-pin header  |  (10,5)»(10,11) |
 | 2. SPI microSD_Card Reader |    6-pin header  | (10,17)»(10,22) |
 | 3. I²C Qwiic dual-ports    |    4-pin header  | (16,14)»(16,17) |
 | 4. I²S INMP441 MEMS Mic    |2x  3-pin headers |  (16,2)»(16,4)  |
 |                            |                  |  (19,2)»(19,4)  |
 | 5. YD-ESP32-S3 (WROOM-1)   |5x 22-pin headers |  (24,3)»(24,24) | •Green•
 |                            |                  |  (25,3)»(25,24) | •Yellow•
 |                            |                  |  (26,3)»(26,24) | •Black•
 |                            |                  |  (35,3)»(35,24) | •Yellow•
 |                            |                  |  (36,3)»(36,24) | •Green•
 | 6. SPI LCD 1.54in 240×240  |    8-pin header  |  (41,8)»(41,15) |

### Vertical Jumper wires: (24)

 |  Color    | Length         | Description      |    Location     |
 |:----------|---------------:|:-----------------|:---------------:|
 | 1. Black  | 4-space jumper | (⏚GND)           |   (2,9)»(6,9)   |
 | 2. Black  | 2-space jumper |                  |  (6,11)»(8,11)  |
 | 3. Black  | 6-space jumper |                  |  (8,12)»(14,12) |
 | 4. Black  | 8-space jumper |                  | (14,13)»(22,13) |
 | 5. Black  | 2-space jumper |                  | (22,11)»(24,11) |
 | 6. Black  | 8-space jumper |                  | (15,42)»(23,42) |
 |           |                |                  |                 |
 | 7. Orange | 4-space jumper | (3V3 VCC)        |  (1,11)»(5,11)  |
 | 8. Orange | 2-space jumper |                  |  (5,12)»(7,12)  |
 | 9. Orange | 8-space jumper |                  |   (7,9)»(15,9)  |
 |10. Orange | 2-space jumper |                  | (15,11)»(17,11) |
 |11. Orange | 2-space jumper |                  |  (1,21)»(3,21)  |
 |12. Orange | 1-space jumper |                  |  (3,28)»(4,28)  |
 |13. Orange |13-space jumper |                  |  (1,42)»(14,42) |
 |           |                |                  |                 |
 |14. Yellow | 3-space jumper | INMP441 WS GPIO4 |  (3,14)»(6,14)  |
 |           |                |                  |                 |
 |15. Green  | 6-space jumper | TFT_RST    GPIO45| (11,40)»(17,40) |
 |           |                |                  |                 |
 |16. Blue   | 4-space jumper | INMP441 SD GPIO6 |  (4,21)»(8,21)  |
 |17. Blue   | 2-space jumper | I²C SDA    GPIO8 | (14,20)»(16,20) |
 |           |                |                  |                 |
 |18. Violet | 7-space jumper | TFT_MOSI   GPIO47| (12,39)»(19,39) |
 |           |                |                  |                 |
 |19. Grey   | 3-space jumper | Vol_DN     GPIO38| (12,55)»(15,55) |
 |           |                |                  |                 |
 |20. Brown  | 7-space jumper | TFT_SCLK   GPIO21| (13,38)»(20,38) |
 |           |                |                  |                 |
 |21. White  | 3-space jumper | OK Button  GPIO0 | (13,56)»(16,56) |
 |           |                |                  |                 |
 |22. White  | 7-space jumper | (horiz)    GPIO38| (11,37)»(11,53) |
 |23. White  | 7-space jumper | (horiz)    GPIO39| (12,37)»(12,53) |
<hr/>

#### IEEE Color Codes

  | Digit | Color   | Code |
  |:-----:|:--------|:----:|
  |  0    | BlacK   |  K   |
  |  1    | BrowN   |  N   |
  |  2    | Red     |  R   |
  |  3    | Orange  |  O   |
  |  4    | Yellow  |  Y   |
  |  5    | Green   |  G   |
  |  6    | Blue    |  B   |
  |  7    | Violet  |  V   |
  |  8    | GrEy    |  E   |
  |  9    | White   |  W   |
  |       |         |      |
  |       | Cyan    |  C   |
  |       | Magenta |  M   |
  |       | Pink    |  P   |
<hr><br/>

### Stripboard Veroboard spacing (Left-side)
--------------------------------------------------------------------
```
Column:          1111111111222222222                    24-pins High
Number: 1234567890123456789012345678 ESP32-S3-WROOM-1   56-pins Wide
--------------------------------------------------------------------
01     3V3 •••→   •  INMP441•         ←-3V3  •Orange•  <--INMP441  ⌷            ALT: SPH0645 MEMS I2S MIC (pinout)
02     GND •••→ • |   ¦o  o¦| Pins    ←-GND⏚ •Black•    L/R¦o  o¦GND                 o 3V3
03    MAX98357  | | Χ•¦ѳ Χo¦• ←⩇⩇⩇ 1. 3V3 ⁊              WS¦o  o¦VIN                 o GND
04      .-------|-| Χ|¦ѳ•Χѳ¦•Χ ⩇⩇⩇ 2. 3V3⮠              SCK¦o  o¦SD                  o BCLK     IO5  Serial Data Clock
05      ¦◯VIN  |ѳ•• |  |   |Χ ⩇⩇⩇ 3.  EN                                            o DOUT     IO6  Serial Data Out
06      ¦  GND  •ѳ•|Χ•  |   | ←⩇⩇⩇ 4. IO4   I²S INMP441  WS   •Yellow•               o LRCL     GND  Left/Right Channel (Left)
07      ¦+ SD   •ѳ|•   Χ•   | ←⩇⩇⩇ 5. IO5   I²S INMP441  SCK  •Green•                o SEL      IO4  Word/Data Select
08      ¦◘ GAIN |ѳ••       Χ• ←⩇⩇⩇ 6. IO6   I²S INMP441  SD   •Blue•
09      ¦◘ DIN  |o |          ←⩇⩇⩇ 7. IO7   I²S MAX98357 DIN
10      ¦– BCLK |o |          ←⩇⩇⩇ 8. IO15  I²S MAX98357 BCLK
11      ¦◯LRC  |o |          ←⩇⩇⩇ 9. IO16  I²S MAX98357 LRC
12      ¯¯¯¯¯¯¯¯|¯ | .----.    ⩇⩇⩇10.◌IO17
13              |  | ¦I²C ¦    ⩇⩇⩇11.◌IO18
14              |  ••¦ ѳX•¦GND←⩇⩇⩇12. IO8   I²C SDA •Blue•
15      microSD •-• |¦ ѳX|¦3V3 ⩇⩇⩇13.◌IO3
16        .-------| |¦Xѳ •XSDA ⩇⩇⩇14.◌IO46I
17        ¦3V3   ѳ• |¦X○  ¦SCL←⩇⩇⩇15. IO9   I²C SCL (•Yellow•)
18     .--¦CS    o  |¦    ¦   ←⩇⩇⩇16. IO10  SPI microSD  CS   (•Orange•)
19     ¦S ¦MOSI  o  |¯¯¯¯¯¯   ←⩇⩇⩇17. IO11  SPI microSD  MOSI (•Yellow•)
20     ¦D ¦SCK   o  |         ←⩇⩇⩇18. IO12  SPI microSD  SCK  (•Green• )
21     \--¦MISO  o  |         ←⩇⩇⩇19. IO13  SPI microSD  MISO (•Blue•  )
22        ¦GND   ѳ•-•X         ⩇⩇⩇20.◌IO14
23        ¯¯¯¯¯¯¯¯|  5V0 •••  ←⩇⩇⩇21. 5V0 X •Red•
24                •  GND •••  ←⩇⩇⩇22. ⏚GND  •Black•
--------------------------------------------------------------------
Column:          1111111111222222222
Number: 1234567890123456789012345678
————————————————————————————————————————————————————————————————————
```
<br/>

### Stripboard Veroboard spacing (Right-side)
--------------------------------------------------------------------
```
Column:             3333344444444445555555              24-pins High
Number:             5678901234567890123456              56-pins Wide
-----------------------------------------------------------------------
01  3V3-->  ESP32-S3-WROOM • •Orange•      »  « Horzontal connect wires
02 ⏚GND-->       Pins      ⏐                    for pins IO39 & IO38
03          GND  44.⩇⩇     |                    on bottom of Veroboard
04 U0TXD҉   IO43◌43.⩇⩇TX   |
05 U0RXD҉   IO44◌42.⩇⩇RX   |LCD 1.54in                  .----------.
06          IO1 ◌41.⩇⩇     |240RGB×240                  ¦TFT Pinout¦
07          IO2 ◌40.⩇⩇    -|-----------.                '----------'
08 TFT_BL   IO42 39.⩇⩇→   o|X TFT_BL   ¦                8. BL   IO42
09 TFT_CS   IO41 38.⩇⩇→   o|X TFT_CS   ¦  Buttons:      7. CS   IO41
10 TFT_DC   IO40 37.⩇⩇→   o|X TFT_DC   ¦                6. DC   IO40
11 Vol_UP   IO39 36.⩇⩇» X•o|X TFT_RST «¦  [◙] Vol_UP⏶  5. RST  IO45
12 Vol_DN   IO38 35.⩇⩇»X•|o|X TFT_SDA «¦• •Grey•        4. MOSI IO47
13 PSRAM    IO37 34.⩇⩇X•||o|X TFT_SCL  ¦|•[◙]  ⏴OK⏵   3. SCLK IO21
14 PSRAM    IO36 33.⩇⩇X|||o•X VCC      ¦||              2. 3V3
15 PSRAM    IO35 32.⩇⩇X|||o•X GND      ¦•|[◙] Vol_DN⏷  1. GND
16 [BOOT]   IO0  31.⩇⩇→|||¯|¯¯¯¯¯¯¯¯¯¯¯¯ • •White•
17 TFT_RST  IO45 30.⩇⩇→||•X| •Green•                    .----------.
18 WS2812҉  IO48◌29.⩇⩇ ||  |                            ¦  Buttons ¦
19 TFT_MOSI IO47 28.⩇⩇→|•X | •Violet•                   '----------'
20 TFT_SCLK IO21 27.⩇⩇→•X  | •Brown•                    Vol_UP  IO39
21 USB D+   IO20◌26.⩇⩇X    |                            Vol_DN  IO38
22 USB D-   IO19◌25.⩇⩇X    |                            Btn_OK  IO0
23          GND  24.⩇⩇ ⏚   • •Black•
24          GND  23.⩇⩇ ⏚                  [◙] RST      ҉RGBLED  IO48
--------------------------------------------------------------------
Column:             3333344444444445555555
Number:             5678901234567890123456
————————————————————————————————————————————————————————————————————
```
<br/><hr>

## 【3】 Quick Wiring Guidelines:

 1. Wiring between ESP32-S3-WROOM-1 Development Board and the &nbsp; ***ST7789 1.54” 240×240 TFT Display:***

> [!TIP]
> ***The Display Pin assignments & Wiring for this project are compatible with:***
> 1. ST7789V2 1.54in 240×240 SPI color IPS TFT Display
> 2. ST7789   1.45in 160×160 SPI color IPS TFT Display
> 3. ST7789V3 1.69in 240×280 SPI color IPS TFT Display (rounded edges)
> 4. ST7789   1.9in  320x170 SPI color IPS TFT Display
> 5. ST7789V  2.0in  320x240 SPI color IPS OLED Display (7-pin) (BL always On)
> 6. ST7789V  2.0in  320x240 SPI color IPS OLED Display (8-pin)

  | ESP32-S3 Dev Board | SPI ST7789 1.54” 240×240 TFT |
  |-------------------:|------------------------------|
  |     GPIO42         | 8. BLK &nbsp; BackLight (BL) |
  |     GPIO41         | 7. CS  &nbsp;&nbsp; Chip Select (CS) |
  |     GPIO40         | 6. RS  &nbsp;&nbsp; Data/Command (DC) |
  |     GPIO45         | 5. RES &nbsp; TFT Reset (RST) |
  |     GPIO47         | 4. SDA &nbsp; SPI Data  (MOSI) |
  |     GPIO21         | 3. SCL &nbsp; SPI Clock (SCLK) |
  |     3V3            | 2. VDD &nbsp; (Power)        |
  |     GND            | 1. GND                       |


 > [SPI LCD 1.54in 240RGB×240 IPS TFT (ST7789) Wiring Diagram](https://github.com/profharris/ESP32-S3-1.54in-TFT-Expansion-Board-with-Speaker/blob/main/images/LCD%201.54in%20240RGB×240%20IPS%20TFT%20(ST7789)%20Wiring%20Diagram.png)

>**Download:**&nbsp; [GFX_ST7789_colorGraphicsDemo.ino &nbsp; (a dozen GFX graphics demos)](https://github.com/profharris/ESP32-S3-1.54in-TFT-Expansion-Board-with-Speaker/blob/main/code/GFX_ST7789_colorGraphicsDemo/GFX_ST7789_colorGraphicsDemo.ino)
<br/>

 2. Wiring between ESP32-S3-WROOM-1 Development Board and the &nbsp; ***I²S INMP441 Microphone:***
    > (The MS3625 chip is Fully Compatible With the INMP441)

  | ESP32-S3 Dev Board  | I²S INMP441 Microphone    |
  |--------------------:|---------------------------|
  |     GND  (LOW=Left) | 1. L/R &nbsp; Left/Right Channel |
  |     GPIO4           | 2. WS  &nbsp;&nbsp; Word/Data Select   |
  |     GPIO5           | 3. SCK &nbsp; Serial Data Clock  |
  |     GPIO6           | 4. SD  &nbsp;&nbsp; Serial Data Out    |
  |     3V3             | 5. VDD &nbsp; (Power)            |
  |     GND             | 6. GND                    |

   > [!TIP]
   > The `L/R pin` &nbsp;(Left/Right) Channel Selection works as follows:<br/>
   >> LEFT &nbsp;– connect L/R to GND.<br/>
   >> RIGHT – connect L/R to VDD.
   
   > [!NOTE]
   > Features:
   > Digital I2S interface with 24-bit accurate data
   > High SNR of 61dBA
   > High sensitivity of -26dBFS
   > Consistent frequency response from 60Hz to 15kHz
   > Low power consumption, low current draw of 1.4mA
   > High PSR of -72 dBFS

> [INMP441 I²S MEMS Mic Wiring Diagram](https://github.com/profharris/ESP32-S3-1.54in-TFT-Expansion-Board-with-Speaker/blob/main/images/INMP441%20I%C2%B2S%20MEMS%20Mic%20Wiring%20Diagram.png)

>**Download:**&nbsp; [INMP441-I2S-Mic-sample.ino &nbsp; (Visual “Audio Waveforms”)](https://github.com/profharris/ESP32-S3-1.54in-TFT-Expansion-Board-with-Speaker/blob/main/code/INMP441-I2S-Mic-sample/INMP441-I2S-Mic-sample.ino)
<br/>

 3. Wiring between ESP32-S3-WROOM-1 Development Board and the &nbsp; ***I²S MAX98357A Amplifier/Speaker:***

  | ESP32-S3 Dev Board | I²S MAX98357A Amplifier/Speaker |
  |-------------------:|---------------------------------|
  |    3V3             | 1. VIN                          |
  |    GND             | 2. GND                          |
  |    VIN (0Ω = Left) | 3. SD   (L/R Channel Select)    |
  |    GND (12dB Gain) | 4. GAIN (Signal Amp)            |
  |    GPIO7           | 5. DIN  Digital Signal In       |
  |    GPIO15          | 6. BCLK Bit Clock               |
  |    GPIO16          | 7. LRC  Left Right Clock        |
  |                    |                                 |
  |    Audio ⧾         | Speaker Positive  (⧾)           |
  |    Audio ⧿         | Speaker Negative  (⧿)           |

> [MAX98357A I²S Audio Amp Wiring Diagram](https://github.com/profharris/ESP32-S3-1.54in-TFT-Expansion-Board-with-Speaker/blob/main/images/MAX98357A%20I²S%20Amp%20Wiring%20Diagram.png)

>**Download:**&nbsp; [1. MAX98357-I2S-Amp-quickTest.ino &nbsp; (440Hz sine wave)](https://github.com/profharris/ESP32-S3-1.54in-TFT-Expansion-Board-with-Speaker/blob/main/code/MAX98357-I2S-Amp-quickTest/MAX98357-I2S-Amp-quickTest.ino)<br/>
>**Download:**&nbsp; [2. MAX98357-I2S-stream-from-Flash.ino &nbsp; _(Stream the “Star Wars” intro)_](https://github.com/profharris/ESP32-S3-1.54in-TFT-Expansion-Board-with-Speaker/blob/main/code/MAX98357-I2S-stream-from-Flash/MAX98357-I2S-stream-from-Flash.ino)<br/>
>**Download:**&nbsp; [&nbsp; &nbsp; StarWars30.h &nbsp; _(“Star Wars” intro)_](https://github.com/profharris/ESP32-S3-1.54in-TFT-Expansion-Board-with-Speaker/blob/main/code/MAX98357-I2S-stream-from-Flash/StarWars30.h)<br/>
>**Download:**&nbsp; [3. MAX98357-I2S-stream-from-PROGMEM.ino &nbsp; _(static)_](https://github.com/profharris/ESP32-S3-1.54in-TFT-Expansion-Board-with-Speaker/blob/main/code/MAX98357-I2S-stream-from-Flash/MAX98357-I2S-stream-from-Flash.ino)<br/>
>**Download:**&nbsp; [&nbsp; &nbsp; soundsample.h &nbsp; _(static)_](https://github.com/profharris/ESP32-S3-1.54in-TFT-Expansion-Board-with-Speaker/blob/main/code/MAX98357-I2S-stream-from-Flash/StarWars30.h)<br/>
>**Download:**&nbsp; [4. ESP32-S3_SD_I2S-MP3Player.ino &nbsp; (I²S MP3 Player)](https://github.com/profharris/ESP32-S3-1.54in-TFT-Expansion-Board-with-Speaker/blob/main/code/ESP32-S3_SD_I2S-MP3Player/ESP32-S3_SD_I2S-MP3Player.ino)<br/>
>**Download:**&nbsp; [&nbsp; &nbsp; Olsen-Banden.mp3 &nbsp;  ESP32-S3_SD_I2S-MP3Player ](https://github.com/profharris/ESP32-S3-1.54in-TFT-Expansion-Board-with-Speaker/blob/main/code/ESP32-S3_SD_I2S-MP3Player/Olsen-Banden.mp3)
<br/>

 4. _(Optional)_&nbsp; Wiring between ESP32-S3-WROOM-1 Development Board and the &nbsp; ***SPI microSD_Card:***

  | ESP32-S3 Dev Board | SPI microSD_Card pins  |
  |-------------------:|------------------------|
  |     3V3            | 1. 3V3  (Power)        |
  |     GPIO10         | 2. CS   &nbsp;&nbsp;(Chip Select)  |
  |     GPIO11         | 3. MOSI &nbsp;(SPI SDI)      |
  |     GPIO12         | 4. CLK  &nbsp;&nbsp;(SPI Clock)    |
  |     GPIO13         | 5. MISO &nbsp;(SPI SDO)      |
  |     GND            | 6. GND                 |

> [!NOTE]
> Micro SD_Card (size: less than 2G), Micro SDHC Card (size: less than 32G).

> [SPI microSD_Card Wiring Diagram](https://github.com/profharris/ESP32-S3-1.54in-TFT-Expansion-Board-with-Speaker/blob/main/images/microSD_Card%20Wiring%20Diagram.jpg)

>**Download:**&nbsp; [1. SPI_SD_Card-Test.ino &nbsp; (SD_card Quick Test)](https://github.com/profharris/ESP32-S3-1.54in-TFT-Expansion-Board-with-Speaker/blob/main/code/SPI_SD_Card-Test/SPI_SD_Card-Test.ino)<br/>
>**Download:**&nbsp; [2. SPI_SD_Card-data_log.ino (SD_card Data Logging Demo)](https://github.com/profharris/ESP32-S3-1.54in-TFT-Expansion-Board-with-Speaker/blob/main/code/SPI_SD_Card-data_log/SPI_SD_Card-data_log.ino)<br/>
>**Download:**&nbsp; [3. SPI_SD_Card-Demo.ino &nbsp; (SD_card Filesystem Demo)](https://github.com/profharris/ESP32-S3-1.54in-TFT-Expansion-Board-with-Speaker/blob/main/code/SPI_SD_Card-Demo/SPI_SD_Card-Demo.ino)
<br/>

 5. _(Optional)_&nbsp; Wiring between ESP32-S3-WROOM-1 Development Board and an I²C interface & OLED Display:<br/>
    (STEMMA-Qt/Qwiic I²C &nbsp;interface)

  | ESP32-S3 Dev Board | I²C 0.96" 128×64 OLED  | Qwiic Wire Color |
  |-------------------:|------------------------|:----------------:|
  |     GND            | 1. GND                 | Black            |
  |     3V3            | 2. VDD (Power)         | Red              |
  |     GPIO8          | 4. SDA I²C Data  (SDA) | Blue             |
  |     GPIO9          | 3. SCL I²C Clock (SCL) | Yellow           |

> [!NOTE]
> There are hundreds of STEMMA-Qt/Qwiic I²C sensors availiable.<br/>
> [Introducing Qwiic / Stemma-Qt](https://learn.adafruit.com/introducing-adafruit-stemma-qt)<br/>
> [Adafruit&nbsp; Stemma-QT/Qwiic I²C sensors & devices](https://www.adafruit.com/category/1018)<br/>
> [SparkFun Stemma-QT/Qwiic I²C sensors & devices](https://www.sparkfun.com/catalogsearch/result/?q=Qwiic)
<br/>

 6. _(Optional)_&nbsp; Wiring between ESP32-S3-WROOM-1 Development Board and additional Buttons:<br/>
    _(Most ESP32-S3 Dev Boards have two Button on-board,&nbsp; **RESET** and **BOOT**)_

  | ESP32-S3 Dev Board | Buttons                                       |
  |-------------------:|-----------------------------------------------|
  |     GPIO39         | **Volume Up**.&nbsp; (shorts to GND), Short press<br/>Increase volume, Long press for Max volume. |
  |     GPIO0          | **BOOT [OK]**.&nbsp; Can also be used as a<br/>“Wake/Interrupt” button. (connected to GND) |
  |     GPIO38         | **Volume Dn**.&nbsp; (shorts to GND), Short press<br/>Reduce volume, Long press Mute.            |

> [!CAUTION]
> The [WeAct Studio ESP32-S3-A/B Core](https://github.com/profharris/ESP32-S3-1.54in-TFT-Expansion-Board-with-Speaker/blob/main/images/3.%20WeAct%20Studio%20ESP32-S3-AB%20Core%20(44-pins).jpg)
> Dev Board already has a _third_ button, at GPIO45 _(TFT Reset)_.

> [Expansion Board with Speaker - Buttons Wiring Diagram](https://github.com/profharris/ESP32-S3-1.54in-TFT-Expansion-Board-with-Speaker/blob/main/images/ESP32-S3%20Expansion%20Board%20with%20Speaker%20Buttons%20Wiring%20Diagram.png)
<hr><br/>

## 【4】 Testing and coding Examples:

—————————————————————————————————————————————————————————————

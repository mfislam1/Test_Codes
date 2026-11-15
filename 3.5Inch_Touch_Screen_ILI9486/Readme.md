3.5 inch TFT TouchScreen Display Shield for Arduino Uno | 480×320 TFT Touch Screen High Quality Display

Contains :-
1. 3.5 inch TFT ILI9486 480×320
2. Touch Screen 
3. 2 X SM245TS
4. 3.3 Voltage regulator 
5. Micro SD Card Slot 


Library Required :- 
MCUFRIEND_kbv.h
Adafruit_GFX.h
TouchScreen.h (Adafruit)


pinOuts :-

const int XP = 6, XM = A2, YP = A1, YM = 7; //ID=0x9341


Portrait Calibration 320 X 480 TS_LEFT = 926, TS_RT = 199, TS_TOP = 954, TS_BOT = 204;

Landscape Calibration 480 x 320  TS_LEFT = 954, TS_RT = 204, TS_TOP = 199, TS_BOT = 926;


Necessary Pins Required to run the Display only :--

## Option A: Direct Connection (Display Only — 8+5 (13) Signal Pins + 3 Power)

Use this wiring if you want maximum drawing speed with the standard `MCUFRIEND_kbv` library while leaving the microSD slot (`D10–D13`) disconnected.

### 1. Control Pins (5 Pins — Analog Header)

| Shield Pin | Arduino UNO Pin | Function |
| :--- | :--- | :--- |
| **LCD_RST** | `A4` | Hardware Reset *(Note: can sometimes be tied to UNO's `RESET` pin to save `A4`, though `MCUFRIEND_kbv` expects `A4` by default)* |
| **LCD_CS** | `A3` | Chip Select (enables the ILI9486 display controller) |
| **LCD_RS** *(or DC)* | `A2` | Register Select / Data-Command toggle *(shared with touch `XM`)* |
| **LCD_WR** | `A1` | Write Strobe *(shared with touch `YP`)* |
| **LCD_RD** | `A0` | Read Strobe *(used by `tft.readID()`, can be tied to `3.3V` if you hardcode `tft.begin(0x9486)`)* |

### 2. 8-Bit Parallel Data Bus (8 Pins — Digital Header)

| Shield Pin | Arduino UNO Pin | Function |
| :--- | :--- | :--- |
| **LCD_D0** | `D8` | Data Bit 0 *(shared with touch `XP`)* |
| **LCD_D1** | `D9` | Data Bit 1 *(shared with touch `YM`)* |
| **LCD_D2** | `D2` | Data Bit 2 |
| **LCD_D3** | `D3` | Data Bit 3 |
| **LCD_D4** | `D4` | Data Bit 4 |
| **LCD_D5** | `D5` | Data Bit 5 |
| **LCD_D6** | `D6` | Data Bit 6 |
| **LCD_D7** | `D7` | Data Bit 7 |

### 3. Power Pins (3 Pins)

* **`5V`**: Powers the onboard 3.3V LDO regulator and the backlight LEDs.
* **`3.3V`**: Logic reference / buffer power on many clones (connect both `5V` and `3.3V` to be safe).
* **`GND`**: Ground.

---

## Option B: Pin-Saving Wiring with 74HC595 (6 Signal Pins + 3 Power)

Use a **74HC595 8-bit shift register** to replace the 8 parallel data wires (`D2–D9`) with 3 hardware SPI pins (`D10`, `D11`, `D13`), freeing up **14 Arduino UNO pins** (`D0–D9`, `A0`, `A4`, `A5`).

### 1. Arduino UNO ↔ 74HC595 (3 SPI Control Pins + Power)

| 74HC595 Pin | Pin Name | Connect To | Function |
| :--- | :--- | :--- | :--- |
| **Pin 14** | `SER` (DS) | **UNO `D11` (MOSI)** | Serial Data Input |
| **Pin 11** | `SRCLK` (SHCP) | **UNO `D13` (SCK)** | Shift Register Clock |
| **Pin 12** | `RCLK` (STCP) | **UNO `D10` (SS)** | Storage / Latch Clock |
| **Pin 10** | `SRCLR` (MR) | **`5V`** | Master Reset (active LOW; tie HIGH to keep enabled) |
| **Pin 13** | `OE` | **`GND`** | Output Enable (active LOW; tie LOW so outputs stay active) |
| **Pin 16** | `VCC` | **`5V`** | Chip Power Supply |
| **Pin 8** | `GND` | **`GND`** | Chip Ground |

### 2. 74HC595 ↔ ILI9486 TFT Shield (8-Bit Parallel Bus)

| 74HC595 Pin | Pin Name | TFT Shield Pin | Function |
| :--- | :--- | :--- | :--- |
| **Pin 15** | `QA` (Q0) | **`LCD_D0`** | Data Bit 0 (LSB) |
| **Pin 1** | `QB` (Q1) | **`LCD_D1`** | Data Bit 1 |
| **Pin 2** | `QC` (Q2) | **`LCD_D2`** | Data Bit 2 |
| **Pin 3** | `QD` (Q3) | **`LCD_D3`** | Data Bit 3 |
| **Pin 4** | `QE` (Q4) | **`LCD_D4`** | Data Bit 4 |
| **Pin 5** | `QF` (Q5) | **`LCD_D5`** | Data Bit 5 |
| **Pin 6** | `QG` (Q6) | **`LCD_D6`** | Data Bit 6 |
| **Pin 7** | `QH` (Q7) | **`LCD_D7`** | Data Bit 7 (MSB) |

### 3. Arduino UNO ↔ ILI9486 TFT Shield (3 Control Pins + Fixed Ties)

| TFT Shield Pin | Connect To | Function |
| :--- | :--- | :--- |
| **`LCD_CS`** | **UNO `A3`** | Chip Select |
| **`LCD_RS`** *(or DC)* | **UNO `A2`** | Register Select (Command / Data) |
| **`LCD_WR`** | **UNO `A1`** | Write Strobe |
| **`LCD_RD`** | **`3.3V`** | Read Strobe tied HIGH (requires calling `tft.begin(0x9486)` directly) |
| **`LCD_RST`** | **UNO `RESET`** | Resets the display whenever the UNO resets |
| **`5V` / `3.3V` / `GND`** | **UNO `5V` / `3.3V` / `GND`** | Display & Backlight Power |

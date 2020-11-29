# Introduction

* Platform: Banana PI M2+ with Armbian
* Emil LUX RF 433.92MHz sockets with remote control - https://www.obi.de/hausfunksteuerung/funkschalter-set-4-teilig-innen-und-aussen/p/3527298
* RF433MHz receiver and sender: https://www.amazon.de/gp/product/B01N5GV39I
  * Sender: FS1000A - 2008-8
  * Receiver: MX-RM-5V - 080408
* Software:
  * https://github.com/blogbasti/BPI-WiringPi (Branch: BPI_M2P)
  * https://github.com/blogbasti/433Utils (Branch: BPI_M2P)

## Connection

* `gpio readall`

 +-----+-----+---------+------+---+---Pi ---+---+------+---------+-----+-----+
 | CPU | wPi |    Name   | Mode | V | Physical | V | Mode |   Name    | wPi | CPU |
 +-----+-----+-----------+------+---+----++----+---+------+-----------+-----+-----+
 |     |     |      3.3v |      |   |  1 || 2  |   |      | 5v        |     |     |
 |  12 |   8 |     SDA.1 | ALT3 | 0 |  3 || 4  |   |      | 5V        |     |     |
 |  11 |   9 |     SCL.1 | ALT3 | 0 |  5 || 6  |   |      | GND       |     |     |
 |   6 |   7 |      PWM1 | ALT3 | 0 |  7 || 8  | 0 | ALT3 | UART3-TX  | 15  | 13  |
 |     |     |       GND |      |   |  9 || 10 | 0 | ALT3 | UART3-RX  | 16  | 14  |
 |   1 |   0 |  UART2-RX | ALT2 | 1 | 11 || 12 | 0 | ALT3 | UART3-CTS | 1   | 16  |
 |   0 |   2 |  UART2-TX |  OUT | 0 | 13 || 14 |   |      | GND       |     |     |
 |   3 |   3 | UART2-CTS | ALT3 | 0 | 15 || 16 | 0 | ALT3 | UART3-RTS | 4   | 15  |
 |     |     |      3.3v |      |   | 17 || 18 | 0 | ALT3 | GPIO.PC04 | 5   | 68  |
 |  64 |  12 | SPI0_MOSI | ALT3 | 0 | 19 || 20 |   |      | GND       |     |     |
 |  65 |  13 | SPI0_MISO | ALT3 | 0 | 21 || 22 | 0 | ALT3 | UART2-RTS | 6   | 2   |
 |  66 |  14 |  SPI0_CLK | ALT3 | 0 | 23 || 24 | 0 | ALT3 | SPI0-CS   | 10  | 67  |
 |     |     |       GND |      |   | 25 || 26 | 0 | ALT3 | GPIO.PL07 | 11  | 71  |
 |  19 |  30 |     SDA.0 | ALT3 | 0 | 27 || 28 | 0 | ALT3 | SCL.0     | 31  | 18  |
 |   7 |  21 | GPIO.PA07 | ALT3 | 0 | 29 || 30 |   |      | GND       |     |     |
 |   8 |  22 | GPIO.PA08 | ALT3 | 0 | 31 || 32 | 0 | ALT3 | GPIO.PL02 | 26  | 354 |
 |   9 |  23 | GPIO.PA09 | ALT3 | 0 | 33 || 34 |   |      | GND       |     |     |
 |  10 |  24 | GPIO.PA10 | ALT3 | 0 | 35 || 36 | 0 | ALT3 | GPIO.PL04 | 27  | 356 |
 |  17 |  25 | SPDIF-OUT | ALT3 | 0 | 37 || 38 | 0 | ALT3 | GPIO.PA21 | 28  | 21  |
 |     |     |       GND |      |   | 39 || 40 | 0 | ALT3 | GPIO.PA20 | 29  | 20  |
 +-----+-----+-----------+------+---+----++----+---+------+-----------+-----+-----+
 | CPU | wPi |    Name   | Mode | V | Physical | V | Mode |   Name    | wPi | CPU |
 +-----+-----+---------+------+---+---Pi ---+---+------+---------+-----+-----+

* Used PINs (physical PINs)

| PIN of 40 PIN header | Pin Sender      | Pin Receiver    |
| ---                  | ---             | ---             |
| 4                    | VCC (right 3)   | VCC (right 4)   |
| 6                    | GND (left 1)    | GND (left 1)    |
| 11    (RX - IN)      | -               | DATA (middle 3) |
| 13    (TX - OUT      | ATAD (middle 2) | -               |

## Codes

* Protocol: 4

| Channel | on       | off      |
| ---     | ---      | ---      |
| A       | 12746048 | 13342608 |
| B       | 12746052 | 13342612 |
| C       | 12746060 | 13342620 |
| D       | 12849938 | 12929330 |

### Example

* channel A on

```bash
./codesend 12746048 4
```

* channel D off 

```bash
./codesend 12929330 4
```



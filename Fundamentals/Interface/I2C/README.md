<img src="/assets/images/interface.png" width="80%" height="80%" />
 
# I2C 

> Connects Components To Microcontrollers Inside Devices

*I2C* (*Inter-Integrated Circuit*, aka *IIC*, Speak: *Eye-Squared-See*) is a synchronous bus system invented 1982 by *Philips* that can transfer data with speeds in the range of *100-400 kBit/s* while requiring just two wires, similar to classic *serial interfaces*. Today, it is widely used to connect *peripherals* like displays or sensors to *microcontrollers* inside devices.

Components requiring higher data rates (such as color displays) typically use [SPI](https://done.land/fundamentals/interface/spi).

## Quick Overview

[I2C](materials/um10204_i2c.pdf) is a two-wire bus that is used to inter-connect components, typically *inside a device* and over a short distance. 


### I2C GPIOs

With *I2C*, there are two pins that need to be connected to your microcontroller:

| Pin Label   | Alternate Labels    |Description |
|---------|-----------------------------------------------------------------------------|----|
| SDA     | DATA, SDI   |Serial Data  (data and target address) |
| SCL     | CLK, SCLK,   |Serial Clock (synchronization)  |


If your microcontroller supports *hardware GPIOs* for *I2C*, it's best to use these pins. They typically offer better performance by supporting higher speeds while reducing the processing burden on the microcontroller. 

When using freely assignable *software GPIOs*, you can assign any available GPIO pin that supports reading and writing. However, using hardware pins is generally recommended for optimal performance.

#### Default I2C Hardware GPIOs for Popular DIY Microcontrollers

Here are the GPIOs **typically** used for hardware *I2C* across some of the popular DIY microcontrollers. 

> [!IMPORTANT]
> The development board **you** are using [may use different GPIOs](https://done.land/components/microcontroller/families/esp/esp32/developmentboards/esp32-c3/c3supermini/#i2c). Modern *ESP32 family members* such as *ESP32-C3* or *ESP32-S3* can route *any* GPIO to its hardware *I2C controllers*, and development board designers may have designated arbitrary *GPIOs*.

| Microcontroller       | SDA Pin        | SCL Pin        | Pin Reassignment Allowed? |
|-----------------------|----------------|----------------|--------------------------|
| **Arduino Uno R3**        | A4   | A5   | No                       |
| **Arduino Uno Mini LE**        | A4   | A5   | No                       |
| **Arduino Uno R4**        | A4   | A5   | No                       |
| **Arduino Uno WiFi**        | 20   | 21   | No                       |
| **Arduino Leonardo**        | D2   | D3   | No                       |
| **Arduino Nano**        | A4   | A5   | No                       |
| **Arduino MKR**        | D11   | D12   | No                       |
| **Arduino Giga R1**        | 20   | 21   | No                       |
| **Arduino Due**        | 20   | 21   | No                       |
| **Arduino Mega 2560**  | 20  | 21                      | No                       |
| **ESP8266**            | 4/D2    | 5/D1    | Yes                      |
| **ESP8266-01**            | 0/D5   | GPIO2/D3   | Yes                      |
| **ESP32**              | 21  | 22  | Yes                      |
| **ESP32-S2**           | 8   | 9   | Yes                      |
| **ESP32-S3**           | 8   | 9  | Yes                      |
| **ESP32-C3**           | 5   | 4   | Yes                      |
| **Teensy**         | [varies](https://www.pjrc.com/teensy/td_libs_Wire.html)  | [varies](https://www.pjrc.com/teensy/td_libs_Wire.html)  | Yes                      |
| **STM32 (Blue Pill)**  | PB9   | PB8  | Yes                      |
| **Digispark (ATTiny85)**| PB0/pin 5  | PB2/pin 7   | No                       |


When you are using a microcontroller that supports *I2C pin reassignments*, make sure you use a freely available *GPIO* that can *read* and *write* and has a built-in pull-up resistor.


### Well-Known I2C Addresses
Before you can access a *I2C* device, you need to know its specific *I2C address*. *I2C* devices typically use a fixed address. Some devices allow you to select an address, i.e. via a series of solder bridges.

If you do not know your devices' *I2C* address, start with a *well-known standard address* for the particular device class. If it is for example a monochrome *OLED display*, use `0x3c`, and if this does not work, try `0x3d`.

If that does not work, or you do not know the well-known *I2C* address for your device class, look up the address in a list:


<details><summary>List of well-known I2C addresses</summary><br/>


| I2C Address | Device Type |
| --- | --- |
| 0x0c | AK8975 |
| 0x0d | AK8975 |
| 0x0e | AK8975, IST-8310, MAG3110 |
| 0x0f | AK8975 |
| 0x10 | LM25066, VEML6075, VEML7700, VML6075 |
| 0x11 | LC709203F, LM25066, SAA5243P/E, SAA5243P/H, SAA5243P/K, SAA5243P/L, SAA5246, Si4713 |
| 0x12 | LM25066, PMSA003I, SEN-17374 |
| 0x13 | LM25066, SEN-17374, VCNL40x0 |
| 0x14 | LM25066 |
| 0x15 | LM25066 |
| 0x16 | LM25066 |
| 0x17 | LM25066 |
| 0x18 | 47L04/47C04/47L16/47C16, COM-15093, LIS3DH, LSM303, MCP9808 |
| 0x19 | COM-15093, LIS3DH, LSM303, MCP9808 |
| 0x1a | 47L04/47C04/47L16/47C16, MCP9808 |
| 0x1b | MCP9808 |
| 0x1c | 47L04/47C04/47L16/47C16, FXOS8700, MCP9808, MMA845x, SAA7706H |
| 0x1d | ADXL345, FXOS8700, MCP9808, MMA845x |
| 0x1e | 47L04/47C04/47L16/47C16, FXOS8700, HMC5883, LSM303, LSM303, MCP9808 |
| 0x1f | FXOS8700, MCP9808 |
| 0x20 | Chirp!, FXAS21002, HW-061, MA12070P, MCP23008, MCP23017, PCA6408A, PCF8574, PCF8575, TCA9554, XD8574A |
| 0x21 | FXAS21002, HW-061, MA12070P, MCP23008, MCP23017, PCA6408A, PCF8574, PCF8575, SAA4700, TCA9554, XD8574A |
| 0x22 | HW-061, MA12070P, MCP23008, MCP23017, PCA1070, PCF8574, PCF8575, TCA9554, XD8574A |
| 0x23 | BH1750FVI, HW-061, MA12070P, MCP23008, MCP23017, PCF8574, PCF8575, SAA4700, TCA9554, XD8574A |
| 0x24 | HW-061, MCP23008, MCP23017, PCD3311C, PCD3312C, PCF8574, PCF8575, TCA9554, XD8574A |
| 0x25 | HW-061, MCP23008, MCP23017, PCD3311C, PCD3312C, PCF8574, PCF8575, TCA9554, XD8574A |
| 0x26 | HW-061, MCP23008, MCP23017, PCF8574, PCF8575, TCA9554, XD8574A |
| 0x27 | HIH6130, HW-061, MCP23008, MCP23017, PCF8574, PCF8575, TCA9554, XD8574A |
| 0x28 | BNO055, CAP1188, DS1841, DS1881, DS3502, FS3000, MCP4532, PM2008 |
| 0x29 | BNO055, CAP1188, DS1841, DS1881, DS3502, MCP4532, TCS34725, TSL2591, VL53L0x, VL6180X |
| 0x2a | CAP1188, DS1841, DS1881, DS3502, MCP4532 |
| 0x2b | CAP1188, DS1841, DS1881, DS3502, MCP4532 |
| 0x2c | AD5248, AD5251, AD5252, CAP1188, CAT5171, DS1881, MCP4532 |
| 0x2d | AD5248, AD5251, AD5252, CAP1188, CAT5171, DS1881, MCP4532, ST25DV16K |
| 0x2e | AD5248, AD5251, AD5252, DS1881, LPS22HB, MCP4532 |
| 0x2f | AD5243, AD5248, AD5251, AD5252, DS1881, MCP4532 |
| 0x30 | SAA2502 |
| 0x31 | SAA2502 |
| 0x32 | ZMOD4410, ZMOD4450 |
| 0x33 | MLX90640, ZMOD4510 |
| 0x36 | MAX17048, MAX17048 |
| 0x38 | AHT10, AHT20, BMA150, FT6x06, PCF8574AP, RRH46410, SAA1064, SEN-15892, VEML6070 |
| 0x39 | APDS-9960, PCF8574AP, SAA1064, TSL2561, VEML6070 |
| 0x3a | MLX90632, PCF8574AP, PCF8577C, SAA1064 |
| 0x3b | PCF8569, PCF8574AP, SAA1064 |
| 0x3c | PCF8569, PCF8574AP, PCF8578, SH1106, SSD1305, SSD1306 |
| 0x3d | PCF8574AP, PCF8578, SH1106, SSD1305, SSD1306 |
| 0x3e | BU9796, PCF8574AP |
| 0x3f | PCF8574AP |
| 0x40 | HDC1080, HTU21D-F, HTU31D, INA219, INA260, LM25066, NE5751, PCA9685, PCF8574, Si7021, TDA8421, TDA9860, TEA6300, TEA6320, TEA6330, TMP006, TMP007 |
| 0x41 | HTU31D, INA219, INA260, LM25066, NE5751, PCA9536, PCA9685, PCF8574, STMPE610, STMPE811, TDA8421, TDA8424, TDA8425, TDA8426, TDA9860, TMP006, TMP007 |
| 0x42 | HDC1008, INA219, INA260, LM25066, PCA9685, PCF8574, TDA8415, TDA8417, TMP006, TMP007 |
| 0x43 | HDC1008, INA219, INA260, LM25066, PCA9685, PCF8574, TMP006, TMP007 |
| 0x44 | HS30xx, INA219, INA260, ISL29125, LM25066, PCA9685, PCF8574, SHT31, STMPE610, STMPE811, TDA4670, TDA4671, TDA4672, TDA4680, TDA4687, TDA4688, TDA4780, TDA8442, TMP006, TMP007 |
| 0x45 | INA219, INA260, LM25066, PCA9685, PCF8574, SHT31, TDA7433, TDA8376, TMP006, TMP007 |
| 0x46 | INA219, INA260, LM25066, PCA9685, PCF8574, TDA8370, TDA9150, TMP006, TMP007 |
| 0x47 | INA219, INA260, LM25066, PCA9685, PCF8574, TMP006, TMP007 |
| 0x48 | ADS1015, ADS1115, ADS7828, INA219, INA260, LM75b, PCA9685, PCF8574, PN532, STDS75, TMP102 |
| 0x49 | ADS1015, ADS1115, ADS7828, AS7262, INA219, INA260, LM75b, PCA9685, PCF8574, STDS75, TMP102, TSL2561 |
| 0x4a | ADS1015, ADS1115, ADS7828, CS43L22, INA219, INA260, LM75b, MAX44009, PCA9685, PCF8574, STDS75, TMP102 |
| 0x4b | ADS1015, ADS1115, ADS7828, INA219, INA260, LM75b, MAX44009, PCA9685, PCF8574, STDS75, TMP102 |
| 0x4c | EMC2101, INA219, INA260, LM75b, PCA9685, PCF8574, STDS75 |
| 0x4d | INA219, INA260, LM75b, PCA9685, PCF8574, STDS75 |
| 0x4e | INA219, INA260, LM75b, PCA9685, PCF8574, STDS75 |
| 0x4f | INA219, INA260, LM75b, PCA9685, PCF8574, STDS75 |
| 0x50 | 47L04/47C04/47L16/47C16, AT24C02N, AT24C64, CAT24C512, FS1015, LM25066, MB85RC, PCA9685 |
| 0x51 | AT24C02N, AT24C64, CAT24C512, LM25066, MB85RC, PCA9685, PCF8563, VCNL4200 |
| 0x52 | 47L04/47C04/47L16/47C16, APDS-9250, AT24C02N, AT24C64, CAT24C512, controller, LM25066, MB85RC, Nunchuck, PCA9685, SI1133 |
| 0x53 | ADXL345, AT24C02N, AT24C64, CAT24C512, LM25066, MB85RC, PCA9685, ST25DV16K |
| 0x54 | 47L04/47C04/47L16/47C16, AT24C02N, AT24C64, CAT24C512, HS40xx, LM25066, MB85RC, PCA9685 |
| 0x55 | AT24C02N, AT24C64, CAT24C512, D7S, LM25066, MAX30101, MB85RC, PCA9685, SI1133 |
| 0x56 | 47L04/47C04/47L16/47C16, AT24C02N, AT24C64, CAT24C512, LM25066, MB85RC, PCA9685 |
| 0x57 | AT24C02N, AT24C64, CAT24C512, LM25066, MAX3010x, MB85RC, PCA9685, ST25DV16K |
| 0x58 | LM25066, PCA9685, SGP30, TPA2016 |
| 0x59 | LM25066, PCA9685, SGP40 |
| 0x5a | CCS811, CCS811, DRV2605, LM25066, MLX90614, MPR121, PCA9685 |
| 0x5b | CCS811, CCS811, MPR121, PCA9685 |
| 0x5c | AM2315, AM2320, BH1750FVI, MPR121, PCA9685 |
| 0x5d | MPR121, PCA9685, SFA30 |
| 0x5e | PCA9685 |
| 0x5f | HTS221, PCA9685 |
| 0x60 | ATECC508A, ATECC608A, MCP4725A0, MCP4725A1, MCP4728, MPL115A2, MPL3115A2, PCA9685, SAB3035, SAB3037, SI1132, Si1145, Si5351A, TEA5767, TSA5511 |
| 0x61 | MCP4725A0, MCP4725A1, MCP4728, PCA9685, SAB3035, SAB3037, SCD30, Si5351A, TEA6100, TSA5511 |
| 0x62 | MCP4725A1, MCP4728, PCA9685, SAB3035, SAB3037, SCD40, SCD40-D-R2, SCD41, TSA5511, UMA1014T |
| 0x63 | MCP4725A1, MCP4728, PCA9685, SAB3035, SAB3037, Si4713, TSA5511, UMA1014T |
| 0x64 | MCP4725A1, MCP4725A2, MCP4728, PCA9685 |
| 0x65 | MCP4725A1, MCP4725A2, MCP4728, PCA9685 |
| 0x66 | IS31FL3731, LTC4151, MCP4725A1, MCP4725A3, MCP4728, PCA9685 |
| 0x67 | LTC4151, MCP4725A1, MCP4725A3, MCP4728, PCA9685 |
| 0x68 | AMG8833, BQ32000, DS1307, DS1371, DS3231, ICM-20948, ITG3200, LTC4151, MCP3422, MPU6050, MPU-9250, MPU-9250, PCA9685, PCF8523, PCF8573, WITTY-PI-3 |
| 0x69 | 3, AMG8833, ICM-20948, ITG3200, LTC4151, MAX31341, MPU6050, MPU-9250, PCA9685, PCF8573, PI, RRH62000, SPS30, WITTY |
| 0x6a | L3GD20H, LTC4151, PCA9685, PCF8573 |
| 0x6b | L3GD20H, LTC4151, PCA9685, PCF8573 |
| 0x6c | LTC4151, PCA9685 |
| 0x6d | LTC4151, PCA9685 |
| 0x6e | LTC4151, PCA9685 |
| 0x6f | LTC4151, MCP7940N, PCA9685 |
| 0x70 | HT16K33, PCA9541, PCA9685, SHTC3, TCA9548, TCA9548A, XD8574 |
| 0x71 | HT16K33, PCA9541, PCA9685, TCA9548, TCA9548A, XD8574 |
| 0x72 | HT16K33, PCA9541, PCA9685, TCA9548, TCA9548A, XD8574 |
| 0x73 | HT16K33, PCA9541, PCA9685, TCA9548, TCA9548A, XD8574 |
| 0x74 | HT16K33, PCA9539, PCA9541, PCA9685, TCA9548, TCA9548A, XD8574 |
| 0x75 | HT16K33, PCA9539, PCA9541, PCA9685, TCA9548, TCA9548A, XD8574 |
| 0x76 | BME280, BME680, BME688, BMP280, HT16K33, MS5607, MS5611, PCA9539, PCA9541, PCA9685, SPL06-007, TCA9548, TCA9548A, XD8574 |
| 0x77 | BMA180, BME280, BME680, BME688, BMP085, BMP180, BMP280, HT16K33, IS31FL3731, MS5607, MS5611, PCA9539, PCA9541, PCA9685, SPL06-007, TCA9548, TCA9548A, XD8574 |
| 0x78 | PCA9685 |
| 0x79 | PCA9685 |
| 0x7a | PCA9685 |
| 0x7b | PCA9685 |
| 0x7c | PCA9685 |
| 0x7d | PCA9685 |
| 0x7e | PCA9685 |
| 0x7f | PCA9685 |


Here is a [detailed list](https://i2cdevices.org/addresses) with clickable device types for more device information.

</details>

### I2C Scanner
You can also create yourself a simple *I2C Address Scanner*: upload the following code to a microcontroller, then connect the unknown device to its designated *I2C pins*. The scanner then scans all devices connected to *I2C*, and lists the addresses it found:

````c++
#include <Arduino.h>
#include <Wire.h>

// change to the GPIOs you want to use for I2C
#define SDA_PIN 21
#define SCL_PIN 22

void setup() {
  Serial.begin(115200);
  delay(2000);
  
  // Initialize I2C with custom pins
  Wire.begin(SDA_PIN, SCL_PIN);

  Serial.println("\nI2C Scanner");
  for (byte address = 1; address < 127; address++) {
    Wire.beginTransmission(address);
    byte error = Wire.endTransmission();
    if (error == 0) {
      Serial.print("I2C device found at address 0x");
      if (address < 16) {
        Serial.print("0");
      }
      Serial.println(address, HEX);
    }
  }
}

void loop() {
  // Nothing to do here
}
````

If you are using [ESPHome](https://done.land/tools/software/esphome/), you can use this configuration to achieve the same:

````
i2c:
  sda: GPIO21
  scl: GPIO22
  scan: True
````



### The Roots: Serial
To understand how *I2C* works and what its requirements are, let's look at where it came from: in some respect, it is a *modernized serial interface*.

The most basic ubiquous interface in computing is the two-wire *serial* interface that many users know:

| Pin | Description |
| --- | --- |
| TX | transmits raw data at defined speed and is connected to RX on the other side  |
| RX | receives raw data at defined speed and is connected to TX on the other side  |

The *serial interface* has severe limitations that make it impractical to connect peripherals in modern devices:

* **1:1 Connection:** it uses a cross-over design and can connect exactly *two* devices. While it is ok to use the *serial interface* with *one* peripheral, you would need distinct serial interfaces *for each* peripheral you want to connect.
* **Asynchronous:** the *serial interface* has no clock signal, so both parties must agree on a given *data transfer speed*, the dreaded *baud rate*. Synchronization is performed on each device independently, so both devices must have a stable clock to not drift away and misinterpret data. This entire concept requires either a *hardware UART* unit, or it is implemented in software and adds significant overhead to the microcontroller.
* **Slow:** because of the overhead, *serial interfaces* are typically limited to a maximum *baud rate* of *230,400 bps* (*28,800 bytes/s*).

The *serial interface* is fairly old which explains some of its design implementations: it stems from a time where signal processing was expensive. That's why *serial* uses separated data lines for *receiving* and *transmitting* which is highly inefficient since most of the time, data transmissions are unidirectional.

With the advent of modern microcontrollers and highly integrated chips, focus could shift from simplistic hardware to sophisticated data protocols, meaning the data carries some of the information that previously was organized by the wires, i.e. *read* and *write* operations.

### I2C Modernizes Serial
*I2C* has the same hardware requirements for the connections, requiring just two wires (and two *GPIOs*):

| Pin | Description |
| --- | --- |
| SDA | Serial Data  (data packages with payload and overhead information, i.e. the target address) |
| SCL | Serial Clock (synchronizes the data speed)  |

However, it more efficiently uses just *one* wire for *bi-directional* data transfer, and uses the other wire for a dynamic clock signal. This way, the data transfer speed can be synchronized, and *I2C* is no longer limited to a pure *1:1* communication. Instead, it is a *bus system* which can improve many limitations found in *serial*:

* **Multiple Peripherals:** *I2C* can address up to *112* peripherals (*clients*)
* **Multiple Controllers:** it supports *multiple controllers* (*masters*)
* **Fast:** it is much faster than *serial* with communication rates in the range of *100-400 kHz*. There are also ultra-fast modes up to *5 MHz* that require higher-quality wiring.
, and since it is designed as a *bus system*, it allows *multiple controllers* within this bus.
* **Simpler:** amazingly, *I2C* is much simpler to implement than *serial* and can be easily implemented in software

Since *I2C* uses cost-effective normal wires without shielding, it is susceptible to noise and can only be used reliably for short distances within a device. Its high impedance requires a *common ground* all devices participating in the bus system.

### Clock

A dedicated *clock line* synchronizes data between the different parties. Communication speed can be negotiated and does not have to be set as a prerequisite (like the *baud rate* in *serial*). Also, any participating device can use the clock to introduce delays, i.e. when it is not yet done processing information.

The clock signal is generated by the (current) bus controller. Slower peripherals can force the clock low in order to signal to the controller that they are not yet ready to receive more data. This is called *clock stretching*.

## Open Drain
Since *I2C* is designed to allow many peripherals and controllers to access the bus *simultaneously*, precautions are necessary to prevent short circuits. Without these safeguards, if one device pulls the bus *low* while another pulls it *high*, harmful currents could flow.

This is why *I2C* uses an *open-drain* configuration *for both lines*, meaning that each participant can actively pull the lines *low*, but cannot pull them *high*. Instead, the line is kept *high by default* through a *pull-up resistor* (typically *4.7kΩ*).

> [!NOTE]
> Both *SDA* and *SCL* need a pull-up resistor.

The resistor value can be adjusted depending on factors like the bus length and the number of devices. For longer cables or higher speeds, you might need to lower the resistance (e.g., 2.2kΩ), while shorter cables may work with higher resistance values.


### Pull-Up Resistor

Issues can arise if a line either has *no pull-up resistor* or *too many*:

- **No Pull-Up Resistor:** If no pull-up resistor is used, the line will be left floating, causing *I2C* communication to fail. This is why many *I2C peripherals* come with pull-up resistors already built in.
- **Too Many Pull-Up Resistors:** When using *I2C peripherals* that include built-in pull-up resistors, their combined resistance may be too low. If the resistance becomes too low, *I2C* communication may fail, and you may experience issues such as excessive current draw.

As a general rule, once you connect more than six *I2C* peripherals with built-in pull-up resistors, the combined resistance may be too low for reliable communication. In most DIY projects, exceeding this number is uncommon.

If you need to use more than six peripherals on a single *I2C bus*, it's important to test each device. Some peripherals with built-in pull-up resistors allow you to *disable* the internal pull-ups, while others may require manual intervention, such as cutting a trace on the PCB.

## I2C Address

*I2C* uses a 7-bit address to uniquely identify individual devices. The address range is from `0x00` to `0x7F` (or `0` to `127` in decimal). 

In theory, this provides *128* unique device addresses, but some addresses are reserved for special purposes. For example, addresses like `0x00` (the general call address) and `0x7F` (reserved for future use) cannot be assigned to devices. This typically leaves 112 usable addresses for devices.

The 8th bit in the address is the *Read/Write (R/W)* bit, which indicates whether the master intends to read from (`1`) or write to (`0`) the slave device.

The maximum number of devices on an *I2C* bus is also limited by *bus capacitance*, which should not exceed *400 pF*. This limit is influenced by factors such as the physical wiring and the number of connected devices.


### Confusing Address Notations

There can be confusion regarding *I2C* device addresses. For example, simple *OLED displays* are often configured with the address `0x3C` by default. However, in some cases, the same address may be specified as `0x78`, leading to uncertainty about which one to use.

In most source codes, the *7-bit* address is used, which in this example is `0x3C`. When *I2C* transmits the address in its *address frame*, the 7-bit address is shifted by one bit to fit into an 8-bit frame in *most significant bit* (MSB) order. This results in the 8-bit value `0x78`.

Thus, while *I2C* **internally** sends the 8-bit value (i.e., `0x78`) over the wire, software (and your source code) should use the original 7-bit address (i.e., `0x3C`).

Aside from this, the address can be specified in different formats. Address `0x3C`, for example, would be `60` in *decimal notation* and `00111100` in *binary* format.


## Protocol
Most of the *I2C* features are defined by its protocol, which is the sequence of data exchanged between devices. Each message is divided into two *frame types*:

* **Address Frame:** This frame targets the recipient by its unique *I2C address* and indicates whether the operation is a *read* or *write* request. A *read* operation requests information from a peripheral, while a *write* operation sends instructions to the peripheral.
* **Data Frame:** This frame contains *8 bits* of data. Multiple *data frames* can be sent, but they are always initiated by an *address frame*.

The sender pulls *SCL low*, then places the frames on the *SDA* line. Once the frames are sent, *SCL* is released and pulled high automatically by the pull-up resistor. This is one of the protocol's key features: the recipient can also pull *SCL low*, indicating that it needs more time to process the data. This prevents the controller from overwhelming a slower peripheral.

### Preventing Collisions
Since *I2C* allows multiple controllers to communicate with multiple peripherals, a smart protocol is used to manage data transmission and prevent collisions:

* **Is Bus Available?** The *SCL* signal tells all participants whether the bus is in use. When the bus is free, *SCL* is *high* (no device is actively pulling it *low*, so the pull-up resistor pulls it *high*).
* **Taking Ownership:** When a controller wants to send data, it actively pulls *SDA* *low*. This signals all peripheral devices that a transmission is about to start. The device that pulls *SDA low* first takes control of the bus.
* **Targeting Device:** The device that gains control begins communication by pulling *SCL low*. It then places the *address frame* on *SDA* in *MSB* format (*most significant bit* first).
* **Establishing Connection:** After sending the address frame, the controller releases *SDA* to *high* and expects the recipient to pull *SDA low* before the 9th clock cycle to acknowledge the transmission.
* **Sending Data:** Once communication is established, the controller continues to generate clock pulses. Depending on whether the address frame indicated a *read* or *write* operation, the peripheral or controller sends one or more *data frames* that are picked up by the counterpart.
* **Releasing Ownership:** After all data frames are transferred, the controller generates a *stop* signal. During normal data writing, *SDA* should never change while *SCL* is *high*. The stop signal first releases *SCL* (which goes *high*), then releases *SDA* (which also goes *high*).


## Voltage
While *I2C* is somewhat resilient to differing signal voltages, it is always safer to use a *logic level shifter* when connecting devices with different voltage levels. For instance, when using a *5V microcontroller board* with *3.3V sensor peripherals*, it’s advisable to add a level shifter to ensure reliable communication.

> [!TIP]
> Ensure you use a *fast* level shifter that supports *I2C*. Basic level shifters may not respond quickly enough to the high signal frequencies used by *I2C*, which could cause communication issues.

## Limitations and Workarounds

Below are some of the most common *I2C* limitations and challenges, along with their corresponding workarounds.

### Issue #1: Fixed Hardware Addresses

Each *I2C* peripheral is identified by a unique address:

* **Device Class:** The simplest approach for many devices in a given class is for them to use the same predefined address. For example, most monochrome *OLED displays* use the address *0x3C* (*0x78* in 8-bit) or *0x3D* (*0x7A* in 8-bit), meaning only one such device can be used on the bus at a time.
* **Jumper:** Some breakout boards feature solder pads that allow you to change the address. Typically, such devices can be configured to use *4-8* different *I2C* addresses, allowing multiple devices of the same class to be used on the same bus as long as each is configured with a unique address.

#### Workaround

If you need to use multiple peripherals that share the same *I2C* address, consider using an *I2C Multiplexer*, such as the *TCA9548A*. A multiplexer can switch between devices, allowing communication with each one individually.



### Issue #2: Unreliable Communication

*I2C* communication is usually very reliable unless there are hardware issues:

* **Pull-Up Resistor:** The bus requires *pull-up resistors*. If you have *no* or *too many* pull-up resistors, *I2C* communication may fail.
* **Distance:** *I2C* is designed for short-range communication within devices, and the optimal distance for reliable operation is typically less than *20 cm*. With longer distances, *I2C* may experience noise interference and weak signals.

#### Workarounds

Ensure that the total pull-up resistance on your *I2C* bus is correct. If you're using more than six peripherals, check if any have built-in pull-up resistors and remove or disable them as needed. If the peripherals lack built-in pull-ups, add them. Start with a *4.7 kOhm* resistor, and lower the resistance if needed, especially for distances greater than *20 cm*.

For distances longer than *20 cm*, use twisted-pair cables and/or shielding to reduce noise, and lower the pull-up resistance. Alternatively, use dedicated *I2C range extenders* that boost the signal strength. If your application requires significantly longer distances, you may want to explore other communication interfaces designed for long-range use, such as *CAN Bus*, which is commonly used in automotive systems.


## Materials

[I2C User Guide](materials/um10204_i2c.pdf)

> Tags: I2C, Address

[Visit Page on Website](https://done.land/fundamentals/interface/i2c?007365071608243322) - created 2024-07-07 - last edited 2025-01-22

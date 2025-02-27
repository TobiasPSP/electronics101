<img src="/assets/images/processor.png" width="80%" height="80%" />
 
# Lilygo T-Display

> ESP32 Development Boards with Integrated Display and Battery Charger

The Chinese company [Lilygo](https://www.lilygo.cc/) produces a variety of *IoT* devices. Its *T-Display* series - also known as *TTGO* - specializes in *ESP32*-based [development boards with integrated displays](https://www.lilygo.cc/collections/basic-module) that all come with an integrated *LiIon battery* connector and charger. *T-Display boards* are a *one-stop* solution for many microcontroller-based *IoT projects*.

<img src="images/lilygo_tdisplay_top_side_t.png" width="60%" height="60%" />

The boards are based on *ESP* microcontrollers, i.e. *ESP32S*, *ESP32-S3*, and *ESP32-C3/6*. The [T-Pico](https://www.lilygo.cc/products/t-pico) *adds* an additional *Raspberry RP2040* to the *ESP* microcontroller (so you can use *both*).

## Overview

Since *Lilygo* has been around for years and has a reputation to defend, the board designs and build quality are excellent. Often more importantly, there is [detailed documentation on GitHub](https://github.com/Xinyuan-LilyGO) as well as helpful after-sales *customer support*.

> [!TIP]
> *T-Display* boards are only slightly more expensive than generic boards (especially when you take into account the added hardware like display and battery charger, and even more so when you take advantage of *special sales* that regularly occur on marketplaces like *AliExpress*).

### Integrated Display
The primary feature of *T-Display development boards* is their integrated display that comes in almost all shapes and technologies.

All boards also come with a connector for a *LiIon* battery: the board can be powered via USB (stationary or off a power bank), or via a dedicated *LiIon battery*. This battery gets automatically recharged once the board is connected to USB power.

Here is a picture of a basic *T-Display* board: the *LiIon* battery can be connected via a *JST 1.25mm* plug on the backside of the board, and an appropriate cable with plug is included:

<img src="images/lilygo_tdisplay_top_horiz_t.png" width="60%" height="60%" />

Models also typically feature programmable buttons, and shells can be ordered separately.

## Models

*T-Display models* vary in features. Use the table below for quick reference: it lists the most commonly used *general purpose* models, their microcontroller(s), and display type and size. For more details, click the model name.


| Name | ESP32 Type | Display Type | Size (Inch)| Resolution | Touch | Driver | 
| --- | --- | --- | --- | --- | --- | --- | 
| [T-Display](https://www.lilygo.cc/products/lilygo%C2%AE-ttgo-t-display-1-14-inch-lcd-esp32-control-board) | ESP32S | Color TFT  | 1.14 | 135x240 | no | ST7789 | 
| [T-Pico](https://www.lilygo.cc/products/t-pico) | C3+RP2040 | Color TFT | 1.14 | 135x240 | no | ST7789 | 
| [T-Pico Pro](https://www.lilygo.cc/products/t-pico) | C6+RP2040 | Color TFT | 2.33 | 222x480 | yes | ST7796 | 
| [T-Display-S3](https://www.lilygo.cc/products/t-display-s3) | S3 | Color TFT | 1.9 | 170x320 | optional | ST7789 | 
| [T-HMI](https://www.lilygo.cc/products/t-hmi) | S3 | Color TFT | 2.8 | 240x320 | yes | ST7789 | 
| [T-Display S3 AMOLED](https://www.lilygo.cc/products/t-display-s3-amoled) | S3 | OLED | 1.91 | 240x536 | optional | RM67162 | 

> [!NOTE]
> There are [many more models](https://www.lilygo.cc/collections/basic-module) available, with additional features such as *LoRa* radio transmitters, or special form factors.



> Tags: Lilygo, T-Display, TTGO, Battery Charger, ESP32, TFT, OLED, RP2040, ESP32S, ESP32-C3, ESP32-C3, ESP32-S3

[Visit Page on Website](https://done.land/components/microcontroller/families/esp/vendorsandseries/lilygot-display?692403090930245026) - created 2024-09-29 - last edited 2024-10-05

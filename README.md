# Tiva
Written by: Mustafa Omareen, February 10, 2023 

The purpose of the article is to present a solution for water and environmental monitoring using the RAK5005-O WisBlock Base, the RAK4630 WisDuo LPWAN module, the Seeed Sensor for Liquid Level Monitoring, and the BMP280 Temperature and Barometric Pressure Sensor. The article provides an overview of the hardware used and the implementation of the solution, including circuit diagrams and power profiling data. 

- [Tiva](#tiva)
- [Bill of Material](#bill-of-material)
- [Run Example](#run-example)
- [Background](#background)
- [Objective](#objective)
- [Hardware](#hardware)
  - [RAK5005-O WisBlock Base](#rak5005-o-wisblock-base)
  - [RAK4630 WisDuo LPWAN Module](#rak4630-wisduo-lpwan-module)
  - [Seeed Sensor for Liquid Level Monitoring](#seeed-sensor-for-liquid-level-monitoring)
    - [Specifications of Seeed sensor:](#specifications-of-seeed-sensor)
    - [Graph representing the relation between output voltage and liquid level (meters):](#graph-representing-the-relation-between-output-voltage-and-liquid-level-meters)
    - [Wiring diagram of Seeed Liquid Level Sensor:](#wiring-diagram-of-seeed-liquid-level-sensor)
  - [BMP280 Temperature and Barometric Pressure Sensor](#bmp280-temperature-and-barometric-pressure-sensor)
- [Implementation](#implementation)
- [Power Profiling](#power-profiling)

# Bill of Material

- [WisBlock Base Board RAK5005-O](https://store.rakwireless.com/products/rak5005-o-base-board?variant=35614952816798)
- [RAK4630 nRF52840 SX1262, LoRa Bluetooth Module for LoRaWAN](https://store.rakwireless.com/products/rak4630-wisduo-lpwan-module?variant=41650213912774)
- [Liquid Level Sensor for Water Level, Oil Level and Mild-corrosive Liquid Level Monitoring](https://www.seeedstudio.com/Liquid-Level-Sensor-p-4619.html)
- [Environment Sensor BOSCH BME680](https://store.rakwireless.com/products/rak1906-bme680-environment-sensor)
- [RAKBox-B2 Enclosure with solar panel](https://store.rakwireless.com/products/rakbox-b2-enclosure-with-solar-panel?variant=39806212047046)
- [Batteri LiPo 3.7V](https://www.electrokit.com/produkt/batteri-lipo-3-7v-1500mah/?gclid=Cj0KCQiAorKfBhC0ARIsAHDzsltIdl01G_LkQJymJZDbWce14WcXJDR_TZrbKWOkfgZBJUKmn6gBCXEaAhd-EALw_wcB)


# Run Example

To run this example, install Visual Studio Code and the PlatformIO extension. Then, follow [this guide](https://docs.helium.com/use-the-network/devices/development/rakwireless/wisblock-4631/platformio/) to add the `nordicnrf52` platform and related boards to your platformio. Once that is done, clone this project and change the `upload_port` to the port your device is connected to. 

```c
[env:wiscore_rak4631]
platform = nordicnrf52
board = wiscore_rak4631
framework = arduino
upload_port = /dev/cu.usbserial-0001
lib_deps = 
	adafruit/Adafruit BMP280 Library@^2.6.6
	beegee-tokyo/SX126x-Arduino@^2.0.15
```

Next, the LoRaWAN keys needs to be changed in `LoRaWAN.cpp`, at lines `70`, `72`, and `74`. 

```c
//  !!!! KEYS ARE MSB !!!!
/** Device EUI required for OTAA network join */
uint8_t nodeDeviceEUI[8] = {0x70, 0xB3, 0xD5, 0x7E, 0xD0, 0x05, 0x9B, 0x6F};
/** Application EUI required for network join */
uint8_t nodeAppEUI[8] = {0x70, 0xB3, 0xD5, 0x7E, 0xD0, 0x05, 0x9B, 0x66};
/** Application key required for network join */
uint8_t nodeAppKey[16] = {0x12, 0x0C, 0x05, 0xFC, 0x88, 0xEC, 0x6D, 0xB0, 0x9E, 0x4F, 0x5E, 0x49, 0xBB, 0xDF, 0x3D, 0x4A};
```

To get these keys, create a new device on The Things Tetwork or Helium. These keys can be randomly generated and will be visible during device creation and on the device page.

![](https://i.imgur.com/3E63sck.png)


Once done, the project can be built and uploaded. The device should soon appear to be joining in the TTN/Helium console. In TTN, they are available under `Activation information`.  

# Background

The RAK5005-O WisBlock Base is a low-power, wireless, and modular solution for Internet of Things (IoT) applications, making it an ideal platform for TiVA's water and environmental monitoring products. The base provides the necessary connectivity options for data collection and processing, including support for WiFi, Bluetooth, and LoRaWAN communication protocols.
The water level sensor from Seeed is used to monitor the water level in rivers, lakes, and other bodies of water, and the BMP280 sensor is used to measure atmospheric pressure and temperature. These sensors provide valuable data about the environment, which is analyzed data for Tiva by using RAK5005-O WisBlock Base to provide real-time insights into the water and atmospheric conditions. 

# Objective
 Providing a powerful and flexible platform for water level and environmental monitoring, allowing TiVA to provide valuable insights to develop innovative technology products for water and environmental monitoring. 

# Hardware
## RAK5005-O WisBlock Base
The main board which is used in this pilot case is RAK5005-O WisBlock Base, this module is built by RAKWireless for the IoT industry. WisBlock modules support dozens of types of CPUs, sensors, and interface circuit boards that allow you to build electronic solutions, through high-speed connectors and easily attachable interconnections, you can be able to build reliable industrial products.

![](https://cdn.shopify.com/s/files/1/0177/8784/6756/products/RAK5005-O_Frontcopy_2c2da7d8-14c5-4990-b43e-35b41d4eca9f_4000x@2x.progressive.png?v=1653490375)


## RAK4630 WisDuo LPWAN Module
The RAK4630 is a low-power long-range transceiver module based on Nordic nRF52840 MCU that supports Bluetooth 5.0 (Bluetooth Low Energy) and the newest SX1262 LoRa transceiver from Semtech. This module complies with Class A, B, & C of LoRaWAN 1.0.3 specifications and also supports LoRa Point-to-Point (P2P) communication mode. The two RF communication characteristic of the module (Lora® + BLE) makes it suitable for a variety of applications in the IoT field, such as home automation, sensor networks, building automation, and IoT network applications.

 
![]( https://www.technoworld.ca/1685/rakwireless-wisblock-lpwan-module-rak4631-ardunio.jpg)


## Seeed Sensor for Liquid Level Monitoring

Seeed Liquid Level Sensor is designed with industry-grade standards, for monitoring water level, oil level, and other mild-corrosive liquid levels. It incorporates stainless steel and insulated rubber is IP68 rated, suitable for application in severe environments. The following figure shows the Seed liquid level sensor.
![](https://sensecap-solution-upload.cdn.seeed.cn/cc/2020/04/water-level-sensor.jpg)

### Specifications of Seeed sensor:
Measurement Range   0 ~ 5 meters
Cable Length    5.3 meters
Output Signal   0.5 ~ 4.5V
Response Time   5 ms
Measurement Liquid  Slight corrosive liquid (water, edible oil, etc.)
Power Supply    5V DC
Compensation Temperature    -10℃ ~ +70℃
Medium Temperature          -40℃ ~ +80℃
Storage Temperature         -40℃ ~ +85℃
special rubber-insulated cable.

### Graph representing the relation between output voltage and liquid level (meters):


![](https://files.seeedstudio.com/products/314990619/img/Output%20Voltage.jpg)


### Wiring diagram of Seeed Liquid Level Sensor:


![](https://files.seeedstudio.com/products/314990619/img/Wiring%20Diagram.jpg)

## BMP280 Temperature and Barometric Pressure Sensor
BMP280 sensor, an environmental sensor with temperature, and barometric pressure is the next generation upgrade to the BMP085/BMP180/BMP183. This sensor is great for all sorts of weather sensing and can even be used in both I2C and SPI!
This sensor from Bosch is the best low-cost, precision sensing solution for measuring barometric pressure with ±1 hPa absolute accuracy, and temperature with ±1.0°C accuracy. Because pressure changes with altitude and the pressure measurements are so good, you can also use it as an altimeter with ±1 meter accuracy.


![](https://www.elfa.se/Web/WebShopImages/landscape_large/9-/01/Adafruit_2651_30129199-01.jpg)




# Implementation
The figure below shows the architecture implementation and the prototype.

![](https://i.imgur.com/91Os1kH.png)


The figure below shows the circuit diagram for the prototype, we used the analog pin (AIN1) and (SCL) (SDA) (VDD)(GND) pins as it is referred to in the figure below.


 ![](https://i.imgur.com/gRFGjcP.png)

# Power Profiling

The power profiling of this prototype is done using an Otii Arc.

The images below show the power consumption of the device when it is joining the network. The sensor wakes up after every two minutes and the required voltage is 3.7 V. 

from time (10sec) to (2min 10sec)  : DS 2min
Imin=-103 uA
Imax=1.68 mA
AVG=86.2 uA
![](https://i.imgur.com/sR3jebf.png)

from time (2min 10sec)  to (2min 17sec) :  DS=7sec
Imin=-603 uA
Imax=97.8 mA
AVG=3.32 mA

![](https://i.imgur.com/WIdEuiV.png)

next cycle
from time (2min 17sec)  to (4min 9sec) :  DS=1min 51sec
Imin=-91.5 uA
Imax=810 uA
AVG=77.7 uA
 
# Orca 3D Tilikum NEMA17 Canbus Contoller
<p align="center">
<a href="./images/Tilikum%20Full%20Size.png" target="_blank"><img src=./images/Tilikum%20Small%20Size.jpg width="500" /></a>
</p>

## Hardware
* MCU: ARM® 32-bit Cortex®-M3 STM32F103RCT6 72MHz with CAN bus
* Stepper Dirver: Onboard TMC2209 in UART mode, UART address: 00, Rsense: 0.1R
* Onboard Accelerometer Sensor: ADXL345
* Input Voltage: DC12V-DC24V Maximum 10A
* Logic Voltage: DC 3.3V
* Heating Interface: Hotend (E0), maximum output current: 6A
* Fan Interfaces: 2 fully controlled fans (FAN0, FAN1)
* Maximum Output Current of Fan Interface: 1A, Peak Value 1.5A
* Expansion Interfaces: 3x EndStops, 1x Bed Probe, 1x RGB Neopixel, 2x Fans, 1x NTC Thermistor, 1x USB interface header, 1x USB interface socket, 1x CAN Interface, 1x SWD programming interface
* Temperature Sensor Interface: 2 Wire NTC Thermistor
* USB Communication Interface: Micro USB or 3 Wire USB Header
* DC 5V Maximum Output Current: 2A

## Firmware
* Only supports Klipper at the present. Currently Marlin doesn't support multiple MCU controllers...

## Pinout
* The following pinout is for thier normally intended common use but mostly can be used for other things inside Klipper for other purposes.
<p align="center">
<a href="./images/Tilikum%20Pinout%20Guide%20Small%20Size.jpg" target="_blank"><img src=./images/Tilikum%20Pinout%20Guide%20Full%20Size.png width="850" /></a>
</p>

## Build Firmware Image
1. Precompiled firmware(Compiled at 28-08-2024)
   * [klipper.bin](./Bootloader%20and%20Firmware/klipper.bin) Klipper build, should be used to program mcu with proper method according to bootloader. USB Build
   * [katapult + klipper.bin](./Bootloader%20and%20Firmware/katapult%20+%20klipper.bin) Bootloader + Klipper, should be programmed using a st-link programmer. USB Build

3. Build your own firmware<br/>
   1. Refer to [klipper's official installation](https://www.klipper3d.org/Installation.html) to download klipper source code to raspberry pi
   2. `Building the micro-controller` with the configuration shown below.
      * [*] Enable extra low-level configuration options
      * Micro-controller Architecture = `STMicroelectronics STM32`
      * Processor model = `STM32F072`
      * IF USE CanBoot
         * Bootloader offset = `8KiB bootloader`
      * ELSE
         * Bootloader offset = `No bootloader`
      * Clock Reference = `8 MHz crystal`
      * IF USE USB
         * Communication interface = `USB (on PA11/PA12)`
      * ElSE IF USE CAN bus
         * Communication interface = `CAN bus (on PB8/PB9)`
         * `(1000000)` CAN bus speed

      <img src=Images/ebb_v10_menuconfig.png width="800" /><br/>
   3. Once the configuration is selected, press `q` to exit,  and "Yes" when  asked to save the configuration.
   4. Run the command `make`
   5. The `klipper.bin` file will be generated in the folder `home/pi/kliiper/out` when the `make` command completed. And you can use the windows computer under the same LAN as raspberry pi to copy `klipper.bin` from raspberry pi to the computer with `pscp` command in the CMD terminal. such as `pscp -C pi@192.168.0.101:/home/pi/klipper/out/klipper.bin c:\klipper.bin`(The terminal may prompt that `The server's host key is not cached` and ask `Store key in cache?((y/n)`, Please type `y` to store. And then it will ask for a password, please type the default password `raspberry` for raspberry pi)

## board.cfg files
   * [sample-bigtreetech-ebb-canbus-v1.0.cfg](./EBB%20CAN%20V1.0%20(STM32F072)/sample-bigtreetech-ebb-canbus-v1.0.cfg) includes all the correct pinout for EBB36 & 42 CAN V1.0


# EBB36 & 42 CAN V1.1 and V1.2
## Hardware
* MCU: ARM Cortex-M0+ STM32G0B1CBT6 64MHz whit FDCAN bus
* All other parameters are the same as EBB36 & 42 CAN V1.0

## Firmware
* Only supports Klipper at the present.

## Pinout
* EBB36 CAN V1.1 and V1.2
  <br/><img src=EBB%20CAN%20V1.1%20and%20V1.2%20(STM32G0B1)/EBB36%20CAN%20V1.1%20and%20V1.2/Hardware/EBB36%20CAN%20V1.1&V1.2-PIN.png width="800" /><br/>
* EBB42 CAN V1.1 and V1.2
  <br/><img src=EBB%20CAN%20V1.1%20and%20V1.2%20(STM32G0B1)/EBB42%20CAN%20V1.1%20and%20V1.2/Hardware/EBB42%20CAN%20V1.1&V1.2-PIN.png width="800" /><br/>

## Build Firmware Image
1. Precompiled firmware(The source code version used is [Commits on Nov 20, 2023](https://github.com/Klipper3d/klipper/commit/bb4711c5d31e8159945f945c662e6668059a174f))
   * [firmware_USB.bin](./EBB%20CAN%20V1.1%20and%20V1.2%20(STM32G0B1)/firmware_USB.bin) Use USB to communicate with raspberry pi.
   * [firmware_canbus.bin](./EBB%20CAN%20V1.1%20and%20V1.2%20(STM32G0B1)/firmware_canbus.bin) Use CAN bus to communicate with raspberry pi, baudrate = 1000K.

3. Build your own firmware<br/>
   1. Refer to [klipper's official installation](https://www.klipper3d.org/Installation.html) to download klipper source code to raspberry pi
   2. Using KIAUH script select these menus: `4. Advanced` -> `2. Build Only` then select as shown below
      * [*] Enable extra low-level configuration options
      * Micro-controller Architecture = `STMicroelectronics STM32`
      * Processor model = `STM32F103`
      * IF USE Katapult or CanBoot
         * Bootloader offset = `8KiB bootloader`
      * ELSE
         * Bootloader offset = `No bootloader`
      * Clock Reference = `12 MHz crystal`
      * IF USE USB
         * Communication interface = `USB (on PA11/PA12)`
      * ElSE IF USE CAN bus
         * Communication interface = `CAN bus (on PB0/PB1)`
         * `(1000000)` CAN bus speed
<p align="center">
<img src=./images/KIAUH%20Firmware%20Guide.jpg width="800" />
</p>
      
   3. Once the configuration is selected, press `q` to exit,  and "Yes" when  asked to save the configuration.
   4. Run the command `make`
   5. The `klipper.bin` file will be generated in the folder `home/pi/kliiper/out` when the `make` command completed. And you can use the windows computer under the same LAN as raspberry pi to copy `klipper.bin` from raspberry pi to the computer with `pscp` command in the CMD terminal. such as `pscp -C pi@192.168.0.101:/home/pi/klipper/out/klipper.bin c:\klipper.bin`(The terminal may prompt that `The server's host key is not cached` and ask `Store key in cache?((y/n)`, Please type `y` to store. And then it will ask for a password, please type the default password `raspberry` for raspberry pi)
   6. If you are using bootloaders like Katapult or CanBoot, easily update the firmware otherwise use an st-link to update the board.

## Firmware update
### Warning: <br/>
(Only for EBB 36/42 CAN V1.1)<br/>
__STM32G0B1CB needs to jump to the System memory area to run bootloader (written by STMicroelectronics) when using DFU to update firmware through the Type-C port. Referring to the description in manual [AN2606](https://www.st.com/content/ccc/resource/technical/document/application_note/b9/9b/16/3a/12/1e/40/0c/CD00167594.pdf/files/CD00167594.pdf/jcr:content/translations/en.CD00167594.pdf), The initialization process of this bootloader is shown in the following figure:__
  <br/><img src=Images/AN2606.png width="800" /><br/>
__The IO of USART will be configured before going to the USB DFU mode.__<br/>
__After going to DFU mode, PA2 will be configured to output high level by bootloader in System memory area refer to the [datasheet of STM32G0B1CB](https://www.st.com/resource/en/datasheet/stm32g0b1cb.pdf)__
  <br/><img src=Images/G0B1_DS.png width="800" /><br/>
__PA2 is used for the hotend MOSFET in EBB36 & 42 CAN V1.1, The high level in the DFU mode change the hotend into heating state. Therefore, please pay attention to disconnect the main power VIN of the hotend when using the DFU of Type-C port to update the firmware, or ensure that the firmware update is completed soon and goto the normal working mode. Never keep MCU in DFU mode for a long time when the main power supply and hotend are connected.__
## board.cfg files
   * [sample-bigtreetech-ebb-canbus-v1.1.cfg](./EBB%20CAN%20V1.1%20and%20V1.2%20(STM32G0B1)/sample-bigtreetech-ebb-canbus-v1.1.cfg) includes all the correct pinout for EBB CAN 36&42 V1.1


# Note
## Here’s BIGTREETECH! For Makers, by makers!
* We appreciate all of your support to BIGTREETECH! To offer an excellent experience of creation to every makers,We’re devoted to design and produce high-quality and durable accessories!

## How to contact:
### If you have any technical issue,please don’t hesitate contact us:
* BIGTREETECH: service004@biqu3d.com

### Follow us on social media to get more news:
* Facebook: https://www.facebook.com/BIGTREETECH/
* Twitter: https://twitter.com/BigTreeTech
* Instagram: https://www.instagram.com/bigtreetech_official/
* Official Site: https://bigtree-tech.com/

## Purchase link:
https://www.biqu.equipment/products/bigtreetech-ebb-36-42-can-bus-for-connecting-klipper-expansion-device

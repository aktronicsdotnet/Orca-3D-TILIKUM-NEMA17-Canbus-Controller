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
         * Communication interface = `CAN bus (on PB8/PB9)`
         * `(1000000)` CAN bus speed
<p align="center">
<img src=./images/KIAUH%20Firmware%20Guide.jpg width="800" />
</p>

   4. Once the configuration is selected, press `q` to exit,  and "Yes" when  asked to save the configuration.
   5. Run the command `make`
   6. The `klipper.bin` file will be generated in the folder `home/pi/kliiper/out` when the `make` command completed. And you can use the windows computer under the same LAN as raspberry pi to copy `klipper.bin` from raspberry pi to the computer with `pscp` command in the CMD terminal. such as `pscp -C pi@192.168.0.101:/home/pi/klipper/out/klipper.bin c:\klipper.bin`(The terminal may prompt that `The server's host key is not cached` and ask `Store key in cache?((y/n)`, Please type `y` to store. And then it will ask for a password, please type the default password `raspberry` for raspberry pi)
   7. If you are using bootloaders like Katapult or CanBoot, easily update the firmware otherwise use an st-link to update the board.

## board.cfg files
   * [test-tilikum-controller.cfg](./Config%20Files/test-tilikum-controller.cfg) includes all the correct pinout for Tilikum Canbus Controller.
      ```
      Don't forget to change your own serial ID in line 19
      serial: /dev/serial/by-id/usb-Klipper_stm32f103xe_36FFD7054741343816842043-if00
      Replace "usb-Klipper_stm32f103xe_36FFD7054741343816842043-if00" with your board ID
      You can get this ID using KIAUH script menu "4. Advanced" -> "5. Get MCU ID" then use correct port "USB or Canbus"
      ```

# Note
## We appreciate your participation
* We appreciate your participation so please inform us if you have any problem or questions.

## Purchase link:
https://eshop.eca.ir

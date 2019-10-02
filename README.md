# IoT Sample

Overall product will upload sensor information unto cloud servers. This project will demonstrate the capability as a stepping stone to develop more advance products.

Project consists of the embedded software that will be developed for installation on a commercial of the shelf (COTS) single board computer that contains a; microcontroller, sensors, USB connectivity, and Wi-Fi connectivity.
* The USB interface will implement a command and response interface that connects directly to a serial terminal (e.g. YAT, HyperTerminal). This will provide the ability to configure the embedded software for different Wi-Fi installations, cloud server configuration, as well the ability to directly connect and monitor system behavior.
* The Wi-Fi interface will provide an Internet connection to periodically deliver sensor information to cloud servers

## Documentation

Project specific software documenation is available [here](https://github.com/abonneville/IoT-Sample/tree/master/Docs):
* Software Requirements Specification (SRS)
* Software Design Documenation (SDD)

## Code Structure

* `Application` project folder implements the thread objects for TCP & UDP communication, and command & response interface via USB
* `CppUTest-Target` contains unit testing performed on the target platform.
* `FreeRTOS` contains FreeRTOS implementation, subdirectory structure is the standard FreeRTOS structure. 
   - `FreeRTOS\Source\portable\GCC\ARM_CM4F` contains the STM32 port.
* `HAL-Extension` The HAL Extension is not generated by the STM32CubeMX tool, and is a design choice to provide a dedicated module that binds and redirects various modules together. The overall goal of the HAL Extension is to provide a porting layer that simplifies maintenance and future adaptations.
* `Network` project folder contains the driver, middleware, and wrapper for implementing TCP & UDP over WiFi interface.
* `Startup` project folder instantiates thread objects and launches the RTOS scheduler.
* `STM32CubeMX` project folder contains the driver and hardware abstraction layer generated by ST's STM32CubeMX tool; USB driver code, clock configuraiton, interrupt configuration, etc...

## Quick Start

This demo project was built for the STM32L475VG and installed on the B-L475E-IOT01A1 Discovery Board.

* Install [ST Microelectronics STM32CubeIDE](https://www.st.com/en/development-tools/stm32cubeide.html), v1.0.2 or later.

    (Other Eclipse based toolchains may also work, as long as a gcc cross-compiler is available for STM32.)

* Clone the IoT Sample project into a local folder (e.g. IoT-Sample)

* Launch Eclipse into your new workspace folder (e.g. IoT-Sample). Import each of the Eclipse projects, see below for list under Code Structure.

* Build the "Startup" project. Load the "Startup.elf" using your preferred debugger (J-Link or ST-Link). 

Note: there are no cloud credentials or WiFi settings specficied in the code.

* Next, install the ST Virtual Com Port (VCP) and connect a USB cable between the PC and device.

* Using a terminal of your choice (e.g. YAT), type "help" to see a list of commands.

* To set cloud credentials, use the following commands:

```
cloud cert
-----BEGIN CERTIFICATE-----
MIIDWTCCAkGgAwIBAgIUK5ZGOMTSz5znkHEp2vJE4oRFiBQwDQYJKoZIhvcNAQEL
....
a36RoKBJ49P/5ftaX2tKqP0yFIvdtcL8cwZtyd/d80/2M3vG9X4o/id/fIpu
-----END CERTIFICATE-----

cloud key
-----BEGIN RSA PRIVATE KEY-----
MIIEowIBAAKCAQEAxoH+pMAciyCycaPTN/MX8bDo0L1DxjpxrNI8E3NaLE31UpTh
...
36tHD3rQlkcQNDYvSW59kwK4Uz/JCJGwb/WXYagnqK0W4nqrccu2
-----END RSA PRIVATE KEY-----


cloud url FromYourAcct.iot.us-west-2.amazonaws.com

```

* To set WiFi settings, use the following commands:
```
wifi password MyPassword
wifi ssid MySsid
```

* Reset your target board, and the terminal should begin displaying progress messages as it connects & publishes sensor
data to AWS cloud server.

To download the latest Wi-Fi firmware (Inventek ISM 43362), see [ STM32L4 Discovery kit IoT node, low-power wireless, Bluetooth Low Energy, NFC, SubGHz, Wi-Fi.](https://www.st.com/resource/en/utilities/inventek_fw_updater.zip) In Binary Resources, choose the download link for Inventek ISM 43362 Wi-Fi module firmware update.

For a basic intorduction getting your devices AWS certificate and keys. try [AWS IoT Hands-On — A Practical Tutorial](https://medium.com/@jankammerath/aws-iot-hands-on-a-practical-tutorial-db8896da5302), scroll down to the section Creating your first Thing in AWS IoT.

## Open Source Components

* [FreeRTOS](http://www.freertos.org/) V10.2.0
* [newlib](https://github.com/mirror/newlib-cygwin) v2.5.0 Note: installed with Atollic TrueStudio

## Licensing

* BSD license (as described in LICENSE) applies to original source files.

* FreeRTOS (since v10) is provided under the MIT license. License details in files under FreeRTOS dir. FreeRTOS is Copyright (C) Amazon.

* Newlib is covered by several copyrights and licenses, as per the files in the `libc` directory.

* [mbedTLS](https://tls.mbed.org/) is provided under the Apache 2.0 license as described in the file extras/mbedtls/mbedtls/apache-2.0.txt. mbedTLS is Copyright (C) ARM Limited.



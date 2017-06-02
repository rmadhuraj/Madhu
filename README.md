[![OpenThread][ot-mad]][ot-repo]  
[![Build Status][ot-travis-svg]][ot-travis]
[![Build Status][ot-appveyor-svg]][ot-appveyor]
[![Coverage Status][ot-codecov-svg]][ot-codecov]

---

# What is OpenThread?  

OpenThread is...
<a href="http://threadgroup.org/technology/ourtechnology#certifiedproducts">
<img src="https://cdn.rawgit.com/openthread/openthread/ab4c4e1e/doc/images/certified.svg" alt="Thread Certified Component" width="150px" align="right">
</a>

**...an open-source implementation of the [Thread](http://threadgroup.org/technology/ourtechnology) networking protocol.** Nest has released OpenThread to make the technology used in Nest products more broadly available to developers to accelerate the development of products for the connected home.

**...OS and platform agnostic**, with a narrow platform abstraction layer and a small memory footprint, making it highly portable.

**...a Thread Certified Component**, implementing all features defined in the [Thread 1.1.1 specification](http://threadgroup.org/technology/ourtechnology#specifications). This specification defines an IPv6-based reliable, secure and low-power wireless device-to-device communication protocol for home applications.

More information about Thread can be found on [threadgroup.org](http://threadgroup.org/).

[thread]: http://threadgroup.org/technology/ourtechnology
[ot-repo]: https://github.com/rmadhuraj/Madhu
[ot-logo]: doc/images/openthread_logo.png
[ot-travis]: https://travis-ci.org/openthread/openthread
[ot-travis-svg]: https://travis-ci.org/openthread/openthread.svg?branch=master
[ot-appveyor]: https://ci.appveyor.com/project/jwhui/openthread
[ot-appveyor-svg]: https://ci.appveyor.com/api/projects/status/r5qwyhn9p26nmfk3?svg=true
[ot-codecov]: https://codecov.io/gh/openthread/openthread
[ot-codecov-svg]: https://codecov.io/gh/openthread/openthread/branch/master/graph/badge.svg
[ot-nordic-img]: directory/nordic.png
[ot-evb1000-img]: directory/evb1000.png
[ot-evb-nordic]: directory/evb-nordic.png
[ot-mad]: nordic.png
# Get started with OpenThread

<a href="https://codelabs.developers.google.com/codelabs/openthread-simulation/index.html">
<img src="doc/images/ot-codelab.png" alt="OpenThread Codelab" width="300px" align="right">
</a>

Want to try OpenThread? The quickest way to get started is to run through our [Simulation Codelab](https://codelabs.developers.google.com/codelabs/openthread-simulation/index.html), which covers all the basics, without the need for test hardware. Using VirtualBox and Vagrant on a Mac or Linux machine, you will learn:

* How to set up the OpenThread build toolchain
* How to simulate a Thread network
* How to authenticate Thread nodes with Commissioning
* How to use `wpantund` to manage a simulated Thread network featuring an NCP

### Next Steps

The Codelab shows you how easy it is use to OpenThread to simulate a Thread network. Once complete:

1. Learn more about the [OpenThread architecture and features](#openthread-features)
1. Get familiar with [platforms and devices that support OpenThread](#who-supports-openthread)
1. See what [testing tools](#what-tools-are-available-for-testing) are available
1. Learn [where to get help](#need-help) and [how to contribute](#want-to-contribute) to the ongoing development of OpenThread

# OpenThread Features

OpenThread implements all features defined in the [Thread 1.1.1 specification](http://threadgroup.org/technology/ourtechnology#specifications), including all Thread networking layers (IPv6, 6LoWPAN, IEEE 802.15.4 with MAC security, Mesh Link Establishment, Mesh Routing) and device roles.

OpenThread supports both system-on-chip (SoC) and network co-processor (NCP) designs. Other features and enhancements include:

* Application support and services
    * IPv6 configuration and raw data interface
    * UDP sockets
    * CoAP client and server
    * DHCPv6 client and server
    * DNSv6 client
    * Command Line Interface (CLI)
* NCP support
    * Spinel - general purpose NCP protocol
    * `wpantund` - user-space NCP network interface driver/daemon
    * Sniffer support via NCP Spinel nodes



# OpenThread on nRF52840-DW1000 Example
## 1. Hardware setup
### Interfacing EVB1000 with Nordic nRF52840 pdk
 |PIN|EVB1000|NRF52840|
 |-----|-----|-----|
 |IRQ|J6: Pin4|P0.30|
 |MISO|J6: Pin5|P0.28|
 |MOSI|J6: Pin8|P0.4|
 |SCLK|J6: Pin7|P0.3|
 |CS|J6: Pin9|P0.29|
 |GND|J6: Pin10|GND|
 |VDD|J10: Pin2|VDD|

### Connectors for SPI Interface on NRF52840 Nordic Platform to EVB1000

[![Nordic][ot-nordic-img]][ot-repo]

### Connection Details of SPI Interface on NRF52840 Nordic Platform

[![EVB][ot-evb1000-img]][ot-repo]

### Connecting Nordic Hardware to EVM with SPI Interface

[![SPI][ot-evb-nordic]][ot-repo]

## Toolchain

Install the GNU ARM Embedded Toolchain

```bash
$ sudo add-apt-repository ppa:team-gcc-arm-embedded/ppa
$ sudo apt-get update
$ sudo apt-get install gcc-arm-embedded
```
(or) 

Run the `setup.sh` script available in `scripts` directory to Install same tool chain
```bash
$ cd scripts
$ sudo chmod 0777 setup.sh
$ sudo ./setup.sh
```
## Building the examples
### 1. The some of source files need executable permissions
 ```bash
$ cd <path-to-openthread-master>
$ cd scripts
$ sudo chmod a+x permissions.sh build.sh
$ ./permissions.sh
```

### 2. Start compiling
```bash
$ cd <path-to-openthread-master>
$ make -f examples/Makefile-nrf52840-dw1000
```
(or)
Run the `build.sh` script available in `scripts` directory
 ```bash
$ cd <path-to-openthread-master>
$ cd scripts
$ sudo chmod a+x build.sh
$ ./build.sh
```

### 3. Converting into Hex format
After a successful build, the `elf` files can be found in
`<path-to-openthread-master>/output/bin`.  You can convert them to `hex`
files using `arm-none-eabi-objcopy`:

```bash
$ cd output/bin
$ arm-none-eabi-objcopy -O ihex arm-none-eabi-ot-cli-ftd arm-none-eabi-ot-cli-ftd.hex
$ arm-none-eabi-objcopy -O ihex arm-none-eabi-ot-ncp arm-none-eabi-ot-ncp.hex
```

## Flashing the binaries
Install the SEGGER-JLink and Command line tools for flashing binary on to nrf52840
#### SEGGER-JLink:
Download the suitable package from [SEGGER-JLink][SEGGER-JLink].
  [SEGGER-JLink]: https://www.segger.com/downloads/jlink
(or)
For Ubuntu 64-bit sytem download [SEGGER-JLink-Ubuntu-64bit][SEGGER-JLink-Ubuntu-64bit]
  [SEGGER-JLink-Ubuntu-64bit]: https://www.segger.com/downloads/jlink/JLink_Linux_V614h_x86_64.deb

Install the package through command line
```bash
$ sudo dpkg -i JLink_Linux_V614h_x86_64.deb
```
### Command line tools
Flash the compiled binaries onto nRF52840 using `nrfjprog` which is
part of the [nRF5x Command Line Tools][nRF5x-Command-Line-Tools].

[nRF5x-Command-Line-Tools]: https://www.nordicsemi.com/eng/Products/nRF52840#Downloads

Download the suitable package. For Ubuntu 64 bit system choose "nRF5x-Command-Line-Tools-Linux64" package from [nRF5x Command Line Tools][nRF5x-Command-Line-Tools].
Extract the tools preferably inside the openthread-master directory which is conveninent to flash.

```bash
$ tar -xvf nRF5x-Command-Line-Tools_9_3_1_Linux-x86_64.tar
$ cd nrfjprog
$ nrfjprog -f nrf52 --chiperase --program ../output/nrf52840/bin/ot-cli-ftd.hex
$ nrfjprog -f nrf52 -r
```

## Running the example

1. Prepare two boards with the flashed `CLI Example` (as shown above).
2. The CLI example uses UART connection. 
   To view raw UART output, start a terminal emulator like PuTTY and connect to the used COM port with the following UART settings:
    - Baud rate: 115200
    - 8 data bits
    - 1 stop bit
    - No parity
    - HW flow control: RTS/CTS
   (or) 
   Use Run the pyterm script provided in the `openthread-master/tools/pyterm` directory.
   On Linux system a port name should be called e.g. `/dev/ttyACM0` or `/dev/ttyACM1`.
3. Open a terminal connection on the first board and start a new Thread network.

 ```bash 
$ cd openthread-master/tools/pyterm
$ sudo ./pyterm -p /dev/ttyACM0
  
 > channel 5
 Done
 > panid 0xdeca
 Done
 > ifconfig up
 Done
 > thread start
 Done
 ```

4. After a couple of seconds the node will become a Leader of the network.

 ```bash
 > state
 Leader
 ```

5. Open a terminal connection on the second board and attach a node to the network.

 ```bash
$ cd openthread-master/tools/pyterm
$ sudo ./pyterm -p /dev/ttyACM1

 > channel 5
 Done
 > panid 0xdeca
 Done
 > ifconfig up
 Done
 > thread start
 Done
 ```

6. After a couple of seconds the second node will attach and become a Child.

 ```bash
 > state
 Child
 ```

7. List all IPv6 addresses of the first board.

 ```bash
 > ipaddr
 fdde:ad00:beef:0:0:ff:fe00:fc00
 fdde:ad00:beef:0:0:ff:fe00:9c00
 fdde:ad00:beef:0:4bcb:73a5:7c28:318e
 fe80:0:0:0:5c91:c61:b67c:271c
 ```

8. Choose one of them and send an ICMPv6 ping from the second board.

 ```bash
 > ping fdde:ad00:beef:0:0:ff:fe00:fc00
 16 bytes from fdde:ad00:beef:0:0:ff:fe00:fc00: icmp_seq=1 hlim=64 time=8ms
 ```

For a list of all available commands, visit [OpenThread CLI Reference README.md][CLI].

[CLI]: https://github.com/openthread/openthread/blob/master/src/cli/README.md

### Directory Structure

The OpenThread repository is structured as follows:

Folder   | Contents
--------------|----------------------------------------------------------------
`doc`         | Spinel docs and Doxygen build file
`etc`         | Configuration files for other build systems (e.g. Visual Studio)
`examples`    | Sample applications and platforms demonstrating OpenThread
`include`     | Public API header files
`src`         | Core implementation of the Thread standard and related add-ons
`tests`       | Unit and Thread conformance tests
`third_party` | Third-party code used by OpenThread
`tools`       | Helpful utilities related to the OpenThread project


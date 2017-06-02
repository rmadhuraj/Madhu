[![OpenThread][ot-logo]][dw-repo]
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
[dw-repo]: https://github.com/rmadhuraj/Madhu
[ot-logo]: DW1000/doc/images/openthread_logo.png
[nordic-img]: DW1000/doc/images/nordic.png
[evb1000-img]: DW1000/doc/images/evb1000.png
[evb-nordic-img]: DW1000/doc/images/evb-nordic.png


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

## Dependent tools to used for Nordic board
### SEGGER-JLink:

Download the suitable package from [SEGGER-JLink][SEGGER].

  [SEGGER]: https://www.segger.com/downloads/jlink
(or)

For Ubuntu 64-bit sytem download [SEGGER-JLink-Ubuntu-64bit][SEGGER-64bit].

  [SEGGER-64bit]: https://www.segger.com/downloads/jlink/JLink_Linux_V614h_x86_64.deb

Install the package by double clicking it
(or)
Install the package through command line
```bash
$ sudo dpkg -i JLink_Linux_V614h_x86_64.deb
```

# Get started with OpenThread

# OpenThread on nRF52840-DW1000 Example
## Hardware setup
The Hardware setup is explained in [Hardware_setup.md][HS].

[HS]: ./DW1000/doc/Hardware_setup.md

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

## Running the CLI example

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

##Running the NCP example
The NCP example is explained in [NCP_Example][NCP].

[NCP]: ./DW1000/doc/NCP_Example.md

## Thread Border Router Example
The Thread Border Router example is explained in [Thread_Border_Router][TBR].

[TBR]: ./DW1000/doc/Thread_Border_Router.md


### KNOWN ISSUES
Observed that Discover Command displays network info all the channels instead of displaying for active channel.

### Known Limitations 
ActiveScan have a limitation of displaying only one node information. During ActiveScan the node will broadcast a message to all the nodes and wait for responses. Since DW1000 doesn't support CSMA/CA if all the responses are arriving in same time, only one node data will be successfully received by the scan initiator node. 

## DOCUMENTATION
* Userguide is available in DW1000/doc/PP_DecaWave_MAC_ReleaseNotes.pdf
* ReleaseNote is available in DW1000/doc/PP_DecaWave_MAC_UserGuide.pdf
* TestReport is available in DW1000/doc/PP_DecaWave_MAC_TestReport.xls

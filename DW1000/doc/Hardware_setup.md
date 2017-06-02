
#Hardware Setup
This section gives how to bring up the Hardware to test Openthread. 

!(./images/evb-nordic.png)

[dw-repo]: https://github.com/rmadhuraj/Madhu
[nordic-img]: DW1000/doc/images/nordic.png
[evb1000-img]: DW1000/doc/images/evb1000.png
[evb-nordic-img]: DW1000/doc/images/evb-nordic.png

[![Decawave-Nordic][evb-nordic-img]][dw-repo]
## Interfacing EVB1000 with Nordic nRF52840 pdk
 |PIN|EVB1000|NRF52840|
 |-----|-----|-----|
 |IRQ|J6: Pin4|P0.30|
 |MISO|J6: Pin5|P0.28|
 |MOSI|J6: Pin8|P0.4|
 |SCLK|J6: Pin7|P0.3|
 |CS|J6: Pin9|P0.29|
 |GND|J6: Pin10|GND|
 |VDD|J10: Pin2|VDD|

## Connectors for SPI Interface on NRF52840 Nordic Platform to EVB1000

[![nRF52840][nordic-img]][dw-repo]

## Connection Details of SPI Interface on NRF52840 Nordic Platform

[![Decawave][evb1000-img]][dw-repo]

## Connecting Nordic Hardware to EVM with SPI Interface

[![Decawave-Nordic][evb-nordic-img]][dw-repo]

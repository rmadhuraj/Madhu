/*TBD for Release*/

# COAP Based Cloud Application:
Nordic designed simple application to send the integer data to the cloud through nrf52840 radio with the coap support in openthread stack. The same is tested with dw1000 radio and it is successfull. 
The setup for this example is same as used in the thread border router example(section 4.3) 
   Steps to be followed to conduct the example are
1. Register with cloud (things.io)
2. Create a new Project
3. Add the token in the CLI source code and recompile the code.
4. Create a widget on the web interface
5. Testing
## 1. Register with cloud (www.things.io)
For a detailed description of the sign-up procedure and an introduction to the web interface of thethings.io, complete the steps described in thethings.io Getting Started.
## 2. Create a new product:
	* Log in to the main control panel. 
	* Go to the Things Manager page. 
	* Create a new product. Select Raspberry Pi as Board
	* Choose JSON as the data type. 
	* Activate the Thing by clicking Activate More Things.
	* After the Thing has been activated, copy the related Thing Token.

## 3. Add the token in the CLI source code and recompile the code.

**Note:** The Token must be copied into the CLI application source code. i.e., The value can be found in the header of the file coap_api.cpp represented as the CLOUD_URI_PATH define. This file located in openthread-master/src/core/api/ directory. Replace Thing Token ({THING_TOKEN} string) with the appropriate one, obtained in the process of Cloud setup.
         	 #define CLOUD_URI_PATH            "v2/things/{THING_TOKEN}"

	EX:#define CLOUD_URI_PATH            "v2/things/DFdOKr5AHo_7Aj-L7UNnIO4BSunTvQeaJSgCWILuYA0"

	1. After replacing the token, recompile the code with enabling coap support in the source code.
			COAP=1 ENABLE_DW1000=1 make -f examples/Makefile-nrf52840-dw1000
	2. Flash the CLI on one node and NCP on another node and connect to the Raspberry Pi as mentioned 		in 4.3 section and start the thread border router setup. 
	3. On cli node by using dwcoapsend command Push the first integer value to the cloud
			dwcoapsend 45
## 4. Create a Widget on the web Interface to monitor the interger values
Go to the Dashboard page.
	1. Optionally, remove all temporary created widgets by clicking Edit Dashboard.
	2.Click Add Widget (+). 
	3. Fill in the name of the widget, for example Temperature, choose Thing Resource as a Data 		Source, and temp as a Resource.
	4. Use Gauge as Widget Type and check the Realtime check box.
	5. Optionally, you may also fill in the ranges ,units, for example Celsius.
	6. You can observe the the Guage widget with Temparature value as 45.
## 5. Testing:
Now, from the CLI node by continously using dwcoapsend command you can observe the changes on the web interface .
```bash
dwcoapsend 50
dwcoapsend 30
```

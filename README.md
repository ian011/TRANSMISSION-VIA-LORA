# TRANSMISSION-VIA-LORA
Transmitting humidity and temperature readings from remote sites to a central hub using LORA

DATA TRANSMISSION VIA LORA
Developing software to collect data from sensors connected to the [NUCLEO F446RE] board. On collection,the stream of data is transmitted to a central hub using lora (a long range low power sensor system ideal for the internet of things and transmission of small bits of data e.g. sensor readings)

prerequisites:

	1. Install Python 3.  (Preferably anaconda) 
	2. Sign up for an Mbed account [https://os.mbed.com/account/signup/]
	3. Clone this repository and cd into it
	4. Create a virtual environment: python3 -m venv ttn
	5. Activate it On Windows: ttn\Scripts\activate.bat
	6. Install the requirements pip install -r requirements.txt
	7. Install InfluxDB following the steps highlighted here:
			https://docs.google.com/document/d/1_5_oCk_9x3BNI6-9pCWj46zaXa0hJ2BBzWs-c3ZNUz8/edit
	8. Install software to obtain console output - (needed to see the output from the microcontrollers)
		 software needed (for windows devices)are:
			a. ST Link - serial driver for the board.
						Run: dpinst_amd64 on 64-bits Windows, dpinst_x86 on 32-bits Windows.
			b. Tera term - to see debug messages from the board.
					https://osdn.net/projects/ttssh2/downloads/66361/teraterm-4.92.exe/
					
Firmware

Hardware requirements are:

	1. NUCLEO-F446RE
	2. LoRaWAN Transceiver Shield (Custom made for DSA by ARM!)
	3. USB Connector
	4. Temperature sensor
	5. Soil Moisture Sensor

SET UP

1. Go to the NUCLEO-F446RE platform page and click Add to your Mbed compiler.(https://os.mbed.com/platforms/ST-Nucleo-F446RE/)

2. To perform temperature and humidity measurement, connect the hardware

3. On the online compiler, open select_program.h.

4. Set: #define PROGRAM TEST_TEMP

5. Compile

6. binary (.bin) file downloads, use drag-and-drop to copy the file to the NODE_F446RE device

7. We need to view the program output with temperature and humidity values on the console.On Windows, Unplug the board and plug it back in.To verify it is configured correctly, Look in 'Device Manager > Ports (COM & LPT)', should list as STLink Virtual COM Port.

8. Output is temperature and humidity readings.

		data transmission over LoRa
a. Log in to the The Things Network console. b. Click Applications. c. Click on the session already defined d. Click Devices. e. Click Register device. f. On the register device page:

	1. First click the generate button below 'Device EUI'.
	2. Enter a proper name for your device and click Register.
	3. Click Settings.
	4. Switch to ABP.
	5. Disable (or uncheck) frame counter checks.
	6. Click Save.
	
Configuring the device
Get the device address, network session key and application session key.

1.Click the Copy button next to 'Device Address' to copy to clipboard.

2.Click the < > button of the Network session key and Application session key values to show the value as C-style array.

3.Click the Copy button on the right of the value to copy to clipboard.

4.Paste these keys into the file device_addresses.h in the appropriate sections: i. Put Device Address on the first line, prefixed with 0x! ii. Put Network Session Key on the second line, add ; at the end. iii. Put Application Session Key on the third line, add ; at the end.

static uint32_t DEVADDR = 0x2601112A;
static uint8_t NWKSKEY[] = { 0xD1, 0x8F, 0xB8, 0x4A, 0xB1, 0x1C, 0xAF, 0x3E, 0xBD, 0xC2, 0xB6, 0x84, 0xEF, 0xD4, 0x41, 0xE4 };
static uint8_t APPSKEY[] =  { 0xAF, 0x05, 0x16, 0x6F, 0x17, 0x34, 0xAD, 0xC0, 0x51, 0xD1, 0xE9, 0x7B, 0xF5, 0xFA, 0x33, 0x6E };
1.Connect the temperature sensor.

2.Connect the LoRa sheild on top of the Nucleo board.

3.The correct orientation of the LoRa shield is when all the logos are on the top.

4.On the online compiler, open select_program.h.

5.Set: #define PROGRAM TEMP_TRANSMIT

6.Compile, flash, ...

7.You should see the data on the console also appear on TTN in your device under the data tab.

Writing Data to a Database
TTN does not store data and for us to use the data in any application, we must store it in a database we configure ourselves. Cayenne is a good example as it is well suited to time series data.

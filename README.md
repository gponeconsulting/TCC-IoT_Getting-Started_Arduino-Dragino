# Getting Started: Arduino + LoRa Shield
This guide will walk you through programming an Arduino using a Dragino LoRa shield to send your first message via The Things Network.
## What you will need
To Follow along with this guide you will need the following things:
- An [Arduino Uno](https://www.jaycar.com.au/duinotech-uno-r3-development-board/p/XC4410) or an [Arduino Mega](https://www.jaycar.com.au/duinotech-mega-2560-r3-board-for-arduino/p/XC4420)
- A [Dragino LoRa shield](https://www.jaycar.com.au/arduino-compatible-long-range-lora-shield/p/XC4392) for the Arduino
- A computer to connect to the Arduino and write the code
- A USB A to USB B cable to connect the Arduino to the your computer


You will also need to be in range of a Gateway connected to The Things Network which you can find out about [here](https://www.thethingsnetwork.org/community).


## Step 1 - Physical setup
To set up the device all we need to do is attach the LoRa shield to the Arduino, attach the antenna to the LoRa shield and plug the Arduino into your computer.

- First attach the LoRa Shield to the Arduino by slotting the shields bottom pins into the Arduino ports, ensuring that the pins labels on the shield match with the port labels on the Arduino (eg. the GND pin on the shield enters the GND port on the Arduino).

- Once the LoRa shield is attached to the Arduino take the antenna that came with the LoRa shield and attach it to the antenna connector on the shield and fasten the thumb screw to fix it in place.

- Finally, use the USB port on the Arduino and the USB cable to connect to a computer.

## Step 2 - Setting up the environment

To get started you will first need to install the Arduino IDE which can be downloaded [here](https://www.arduino.cc/en/software).
After downloading the appropriate option for your system, run the installer and complete the installation process.
Once the program is done installing, open the Arduino IDE.

Now that we are in the Arduino IDE the first thing we want to do is select the type of board we are using.
- To do this Navigate to `Tools > Board: > Arduino AVR Boards >` and then select the board that you are using (eg. 'Arduino Uno' or 'Arduino Mega or Mega 2560')

Now that the board has been correctly selected, we must install a library to help use the LoRa shield to connect to The Things Network.

- First go to the libraries github page [here](https://github.com/thomaslaurenson/arduino-lmic)
- Next, Download a .zip file of the library by pressing the green button labeled `code` and selecting `Download ZIP`
- After finishing the download go back the Arduino IDE and navigate to `Sketch > Include Library > Add .ZIP Library...`
- When the selection window opens select the .zip file that you downloaded and click open.

With this the library has been added and we can move on with connecting to The Thing Network.

## Step 3 - Sign Up on The Things Network
Now that we have our environment set up we can prepare to connect to The Things Network by following the steps below.

1. Create an account at [The Things Network](https://account.thethingsnetwork.org/register) or sign in if you already have an account
2. Go to the consol by clicking on the profile icon and clicking the console option.
3. select applications
4. select add application
5. Fill out the application ID with a unique name, add a Description and press `add application`.
6. Press the `register device` button in the devices section.
7. Enter a unique name for the device ID
8. Click the arrow icon on the left of the `Device EUI field` to generate an EUI
9. Click `Register`
Your Arduino will now be registered and is ready to connect


## Step 4 - Connecting The Arduino to The Things Network
Now that the Arduino is registered on The Things Network all that is left to do is connect to the network.
- In the Arduino IDE Navigate to `File > Examples > MCCI LoRaWAN LMIC library > ttn-otaa-dragino-LoRashield-au915` which will open a new window.

In the code that has just been opened in the new window, we will need to change 3 things.

1. Change the APPEUI to the `Application EUI` from your The Things Network Device Overview that you created in step 3.
- To do this, on The Things Network device overview page first press the button labeled `<>` next to the Application EUI field.
- Then click the arrows symbol next to the `<>` symbol to change the code to little-endian format
- Copy the code and paste it into the `FillMEIN` section of the following Arduino code
`static const u1_t PROGMEM APPEUI[8]= { FILLMEIN };`

2. Change the DEVEUI to the `Device EUI` from your The Things Network Device Overview that you created in step 3.
- To do this, on The Things Network device overview page first press the button labeled `<>` next to the Device EUI field.
- Then click the arrows symbol next to the `<>` symbol to change the code to little-endian format
- Copy the code and paste it into the `FillMEIN` section of the following Arduino code
`static const u1_t PROGMEM DEVEUI[8]= { FILLMEIN };`

3. Change the APPKEY to the `App Key` from your The Things Network Device Overview that you created in step 3.
- To do this, on The Things Network device overview page first press the button labeled `<>` next to the App Key field.
- Then copy the code and paste it into the `FillMEIN` section of the following Arduino code
`static const u1_t PROGMEM APPKEY[16] = { FILLMEIN };`

Now everything is set up for connecting to The Things Network.
- Make sure that the serial monitor is closed
- Click the arrow in the upper left corner to upload the code to the Arduino
- After waiting for the code to upload you can now open the Serial Monitor through `Tools > Serial Monitor` and see the output.
- after the Serial Monitor is opened, in the bottom right section of the serial window set the dropdown to `115200 baud`

if everything went well it should post a Successful transmission every 60 seconds which you will also be able to see on the things network website in the device data section.  


## Step 5 - Customising your message
Right now the example code is sending the hex encoded message for `OTAA` which can be seen as `4F 54 41 41` in The Things Network Data tab.
We can change this to something else by changing the text in the line `static uint8_t mydata[] = "OTAA";` in the Arduino IDE.
The time between messages can also be changed by changing the value of the line `const unsigned TX_INTERVAL = 60;` in the Arduino IDE.

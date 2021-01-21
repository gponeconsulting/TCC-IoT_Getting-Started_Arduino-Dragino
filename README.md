# LoRa Shield Guide
## What you will need
To follow this guide, you will need the following:
- An [Arduino Uno](https://www.jaycar.com.au/duinotech-uno-r3-development-board/p/XC4410) or [Arduino Mega](https://www.jaycar.com.au/duinotech-mega-2560-r3-board-for-arduino/p/XC4420)
- A [Dragino LoRa Shield](https://www.jaycar.com.au/arduino-compatible-long-range-lora-shield/p/XC4392) for the Arduino
- A computer to connect to the Arduino and write the code
- A USB A to USB B cable to connect the Arduino to your computer


You will also need to be in range of a Gateway connected to The Things Network which you can find out about [here](https://www.thethingsnetwork.org/community).


## Step 1 - Physical Setup
To set up the device, attach the LoRa Shield to the Arduino, attach the antenna to the LoRa Shield, and plug the Arduino into your computer.

1. First attach the LoRa Shield to the Arduino by slotting the shields bottom pins into the Arduino ports, ensuring that the pins labels on the shield match with the port labels on the Arduino (e.g. the GND pin on the shield enters the GND port on the Arduino).
2. Now take the antenna that came with the LoRa Shield and attach it.
3. Finally, use the USB port on the Arduino and the USB cable to connect to a computer.

## Step 2 - Setting up the Environment

To get started you will first need to install the Arduino IDE which can be downloaded [here](https://www.arduino.cc/en/software).
After downloading the appropriate option for your system, run the installer and complete the installation process.
Once the program is done installing, open the Arduino IDE.

Now that we are in the Arduino IDE, select the type of board you are using.
- To do this, Navigate to Tools > Board: > Arduino AVR Boards > and then select the board that you are using (e.g., 'Arduino Uno' or 'Arduino Mega or Mega 2560')

Now that the board has been correctly selected, install a library to help use the LoRa Shield to connect to The Things Network.

- First, open the library manager by going to `Tools > Manage Libraries`
- Next, in the bar at the top of the window search for `MCCI LoRaWAN LMIC library` and install the library with the same name.
- Click close at the bottom of the window

Now that we have the library installed, configure it to the Australian standard.

- Open the Arduino libraries folder (by default this will be in `Documents/Arduino/libraries`)
- Open `MCCI_LoRaWAN_LMIC_library > project_config > lmic_project_config.h` in notepad or another code editor
- In the file add `//` before the `#define CFG_us915 1` line and remove `//` from the start of the `//#define CFG_au915 1` to change the region

after these changes, the code should look similar to the following:

```
// project-specific definitions
//#define CFG_eu868 1
//#define CFG_us915 1
#define CFG_au915 1
//#define CFG_as923 1
// #define LMIC_COUNTRY_CODE LMIC_COUNTRY_CODE_JP	/* for as923-JP */
//#define CFG_kr920 1
//#define CFG_in866 1
#define CFG_sx1276_radio 1
//#define LMIC_USE_INTERRUPTS
```
- Save the file

The library has now been added. Now to connect to The Things Network.

## Step 3 - Sign Up on The Things Network
Now that our environment is set up, we can prepare to connect to The Things Network by following the steps below.

1. Create an account at [The Things Network](https://account.thethingsnetwork.org/register) or sign in if you already have an account
2. Go to the consol by clicking on the profile icon and clicking the console option.
3. Select applications
4. Select add application
5. Fill out the application ID with a unique name, add a Description and press `add application`.
6. Press the `register device` button in the devices section.
7. Enter a unique name for the device ID
8. Click the arrow icon on the left of the `Device EUI field` to generate an EUI
9. Click `Register`
Your Arduino will now be registered and is ready to connect


## Step 4 - Connecting the Arduino to The Things Network
Now that the Arduino is registered on The Things Network all that is left to do is connect to the network.
- In the Arduino IDE Navigate to `File > Examples > MCCI LoRaWAN LMIC library > ttn-otaa` which will open a new window.

In the code that has just been opened in the new window, we will need to change 4 things:

1. Change the APPEUI to the `Application EUI` from your The Things Network Device Overview that you created in step 3.
- To do this, on The Things Network device overview page first press the button labelled `<>` next to the Application EUI field.
- Then click the arrows symbol next to the `<>` symbol to change the code to little-endian format.
- Copy the code and paste it into the `FillMEIN` section of the following Arduino code
`static const u1_t PROGMEM APPEUI[8]= { FILLMEIN };`

2. Change the DEVEUI to the `Device EUI` from The Things Network Device Overview that you created in step 3.
- To do this, on The Things Network device overview page first press the button labeled `<>` next to the Device EUI field.
- Then click the arrows symbol next to the `<>` symbol to change the code to little-endian format
- Copy the code and paste it into the `FillMEIN` section of the following Arduino code
`static const u1_t PROGMEM DEVEUI[8]= { FILLMEIN };`

3. Change the APPKEY to the `App Key` from your The Things Network Device Overview that you created in step 3.
- To do this, on The Things Network device overview page first press the button labelled `<>` next to the App Key field.
- Then copy the code and paste it into the `FillMEIN` section of the following Arduino code
`static const u1_t PROGMEM APPKEY[16] = { FILLMEIN };`

4. Change the section that is labelled `Pin mapping` to the following code:
```
// Pin mapping for Dragino LoRashield
const lmic_pinmap lmic_pins = {
    .nss = 10,
    .rxtx = LMIC_UNUSED_PIN,
    .rst = 9,
    .dio = {2, 6, 7},
};
```

Now everything is set up for connecting to The Things Network.
- Make sure that the serial monitor is closed
- Click the arrow in the upper left corner to upload the code to the Arduino
- After waiting for the code to upload you can now open the Serial Monitor through `Tools > Serial Monitor` and see the output.
- After the Serial Monitor is opened, in the bottom right section of the serial window set the dropdown to `115200 baud`

If everything went well it should post a Successful transmission every 60 seconds which you will also be able to see on The Things Network website in the device data section.  


## Step 5 - Customising Your Message
Right now the example code is sending the hex encoded message for `Hello, world!` which can be seen as `48 65 6C 6C 6F 2C 20 77 6F 72 6C 64 21` in The Things Network Data tab.
We can change this to something else by changing the text in the line `static uint8_t mydata[] = "Hello, world!";` in the Arduino IDE.
The time between messages can also be changed by changing the value of the line `const unsigned TX_INTERVAL = 60;` in the Arduino IDE.


# Manual for connection between Google assistant and Adruino 
## with the help off Adafruit.IO and IFTT

### What do you need?

1. A device with the google assistant
2. An account for IFTT and the app
3. An account on Adafruit.IOt and knowledge how to create a feed
4. A Adruino with a led strip

### Lets create
1. First create a feed on adafruit
2. Open IFTTT click on "get more" and click on " Make your own Applets from scratch"
3. Choose if and search for the google assistant
4. Choose "Say a simple phrase"
5. Set up what you want to say, and if you want a response back fill that in. (A response can be handy at the start, if you get the right response you know your applet works. 
6. AFter setting up Google assistant, choose that and search for Adafruit.io
7. Choose Adafruit and click on the feed you just made.
8. Give a value, this value can be as simple as "1", we are going to use that value to controll the arduino.
9. Lets test your Applet, talk to an device where you are signed in with your google assistent, say the commando phrase.
10. Do you get the right answer back? Great, lets see your feed at Adafrui.io and check if the "1" value has arrived.


11. Make in ardruino a new project with a "config.h" file 
```bash
/************************ Adafruit IO Config *******************************/

// visit io.adafruit.com if you need to create an account,
// or if you need your Adafruit IO key.
#define IO_USERNAME    "***yourusername***"
#define IO_KEY         "***yourkey***"

/******************************* WIFI **************************************/

// the AdafruitIO_WiFi client will work with the following boards:
//   - HUZZAH ESP8266 Breakout -> https://www.adafruit.com/products/2471
//   - Feather HUZZAH ESP8266 -> https://www.adafruit.com/products/2821
//   - Feather HUZZAH ESP32 -> https://www.adafruit.com/product/3405
//   - Feather M0 WiFi -> https://www.adafruit.com/products/3010
//   - Feather WICED -> https://www.adafruit.com/products/3056

#define WIFI_SSID       "***yourssid***"
#define WIFI_PASS       "***yourpassword***"

// comment out the following two lines if you are using fona or ethernet
#include "AdafruitIO_WiFi.h"
AdafruitIO_WiFi io(IO_USERNAME, IO_KEY, WIFI_SSID, WIFI_PASS);


/******************************* FONA **************************************/

// the AdafruitIO_FONA client will work with the following boards:
//   - Feather 32u4 FONA -> https://www.adafruit.com/product/3027

// uncomment the following two lines for 32u4 FONA,
// and comment out the AdafruitIO_WiFi client in the WIFI section
// #include "AdafruitIO_FONA.h"
// AdafruitIO_FONA io(IO_USERNAME, IO_KEY);


/**************************** ETHERNET ************************************/

// the AdafruitIO_Ethernet client will work with the following boards:
//   - Ethernet FeatherWing -> https://www.adafruit.com/products/3201

// uncomment the following two lines for ethernet,
// and comment out the AdafruitIO_WiFi client in the WIFI section
// #include "AdafruitIO_Ethernet.h"
// AdafruitIO_Ethernet io(IO_USERNAME, IO_KEY);
```


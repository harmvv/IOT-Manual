# Manual for connection between Google assistant and Arduino 
## with the help of Adafruit.IO and IFTT

### What will it do?
You will be able to turn the led light to a specifc color by talking to the Google Assistent

### What do you need?

1. A device with the google assistant.
2. An account for IFTT and the app.
3. An account on Adafruit.IO and knowledge how to create a feed.
4. A Arduino with a led strip.
5. Neopixel libary / MQTT Libary / Adafruit.IO Libary

### Lets create
1. First create a feed on Adafruit.IO, name if whatever you want.
2. Open IFTTT click on "get more" and click on " Make your own Applets from scratch".
3. Choose if and search for the google assistant.
4. Choose "Say a simple phrase".
5. Set up what you want to say, and if you want a response back fill that in. (A response can be handy at the start, if you get the right response you know your applet works. 
6. AFter setting up Google assistant, choose that and search for Adafruit
7. Choose Adafruit and click on the feed you just made.
8. Give a value, this value can be as simple as "1", we are going to use that value to controll the arduino.
Here a example of one applet, for our arduino example i created 4 with the value's 0,1,2,3
![Image that shows IFTTT example](https://oege.ie.hva.nl/~versevh/afbeeldingen/manual2/ifttt.jpg)
9. Lets test your Applet, talk to an device where you are signed in with your google assistent, say the commando phrase.
10. Do you get the right answer back? Great, lets see your feed at Adafrui.io and check if the "1" value has arrived.


11. Make in arduino a new project with a "config.h" file, copy the code below and change the "*...*" to your details. 
![Image that shows IFTTT example](https://oege.ie.hva.nl/~versevh/afbeeldingen/manual2/Mapstructure.png)
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

11. Place the following code in your Arduino file.
12. Connect your led strip to D5.
13. This code works with multiple libaries, check if you have them installed. You can find them at the begin of this tutorial.

``` bash


#include "config.h"
#include <Adafruit_NeoPixel.h>

#ifdef __AVR__
#include <avr/power.h>
#endif


#define PIN     D5
#define NUMPIXELS      10
#define PIXEL_TYPE    NEO_GRB + NEO_KHZ800

Adafruit_NeoPixel pixels = Adafruit_NeoPixel(NUMPIXELS, PIN, NEO_GRB + NEO_KHZ800);

int delayval = 50; // delay for half a second

// set up the 'light1' feed
AdafruitIO_Feed *light1 = io.feed("light1");

void setup() {

  // start the serial connection
  Serial.begin(115200);

  // wait for serial monitor to open
  while (! Serial);

  Serial.print("Connecting to Adafruit IO");

  // start MQTT connection to io.adafruit.com
  io.connect();

  // set up a message handler for the count feed.
  // the handleMessage function (defined below)
  // will be called whenever a message is
  // received from adafruit io.
  light1->onMessage(handleMessage);

  // wait for an MQTT connection
  // NOTE: when blending the HTTP and MQTT API, always use the mqttStatus
  // method to check on MQTT connection status specifically
  while (io.mqttStatus() < AIO_CONNECTED) {
    Serial.print(".");
    delay(500);
  }

  // Because Adafruit IO doesn't support the MQTT retain flag, we can use the
  // get() function to ask IO to resend the last value for this feed to just
  // this MQTT client after the io client is connected.
  light1->get();

  // we are connected
  Serial.println();
  Serial.println(io.statusText());

  pixels.begin(); // This initializes the NeoPixel library.

}

void loop() {

  // io.run(); is required for all sketches.
  // it should always be present at the top of your loop
  // function. it keeps the client connected to
  // io.adafruit.com, and processes any incoming data.
  io.run();

  // Because this sketch isn't publishing, we don't need
  // a delay() in the main program loop.

}

// this function is called whenever a 'light1' message
// is received from Adafruit IO. it was attached to
// the light1 feed in the setup() function above.
void handleMessage(AdafruitIO_Data *data) {

  Serial.print("received <- ");
  Serial.println(data->value());

  String situationvalue = data->toString();
  Serial.print("situation level " + situationvalue);


  if (situationvalue == String("0")) {
    Serial.print(" No problems ");

    for (int i = 0; i < NUMPIXELS; i++) {

      // pixels.Color takes RGB values, from 0,0,0 up to 255,255,255
      pixels.setPixelColor(i, pixels.Color(124, 252, 0)); // Moderately bright green color.

      pixels.show(); // This sends the updated pixel color to the hardware.

      delay(delayval); // Delay for a period of time (in milliseconds).
    }
  }

  else if (situationvalue == String("1")) {
    Serial.print(" We have to check this one ");

    for (int i = 0; i < NUMPIXELS; i++) {

      // pixels.Color takes RGB values, from 0,0,0 up to 255,255,255
      pixels.setPixelColor(i, pixels.Color(255, 165, 0)); // Moderately bright green color.

      pixels.show(); // This sends the updated pixel color to the hardware.

      delay(delayval); // Delay for a period of time (in milliseconds).

    }
  }
  else if (situationvalue == String("2")) {
    Serial.println(" There are problems, someone get over here right now  ");
    Serial.print(situationvalue);
    for (int i = 0; i < NUMPIXELS; i++) {

      // pixels.Color takes RGB values, from 0,0,0 up to 255,255,255
      pixels.setPixelColor(i, pixels.Color(255, 0, 0)); // Moderately bright green color.

      pixels.show(); // This sends the updated pixel color to the hardware.

      delay(delayval); // Delay for a period of time (in milliseconds).

    }

  }

  else if (situationvalue == String("3")) {
    Serial.println(" ALERT ALERT PANIC ALERT ALERT  ");
    Serial.print(situationvalue);
    for (int e = 0; e < 4; e++) {
      for (int i = 0; i < NUMPIXELS; i++) {

        //     pixels.Color takes RGB values, from 0,0,0 up to 255,255,255
        pixels.setPixelColor(i, pixels.Color(255, 0, 0)); // Moderately bright green color.

        pixels.show(); // This sends the updated pixel color to the hardware.
        delay(150);

        // pixels.Color takes RGB values, from 0,0,0 up to 255,255,255
        pixels.setPixelColor(i, pixels.Color(0, 0, 0)); // Moderately bright green color.

        pixels.show(); // This sends the updated pixel color to the hardware.

        pixels.setPixelColor(i, pixels.Color(255, 0, 0)); // Moderately bright green color.

        pixels.show(); // This sends the updated pixel color to the hardware.
        delay(150);
        delay(delayval); // Delay for a period of time (in milliseconds).

      }

    }
  }

}

```

14. Upload it to your Arduino, test the code through talking with the Google assistent. If it its not working check if the value's update on the Adafruit.io website.

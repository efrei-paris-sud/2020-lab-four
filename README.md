# Introduction to Cloud
To communicate with other devices we need to use a board that has WiFi, Bluetooth or etc. 
Therefore we will use ESP 32 board which is suitable for this usage.

# Step 1: Installing ESP32 Add-on in Arduino IDE
Please follow this [link](https://randomnerdtutorials.com/installing-the-esp32-board-in-arduino-ide-windows-instructions/).

# Step 2: Find You ESP32 chipset.
Look at your board. Find a gold chipset. see the name and find this name in the arduino IDE boards.

# Step 3: Upload blinking LED code. 
Remember Lab 1, upload the `Blink LED` source code. build the circut and see if it works.
|Ex.1| Add this step to the report.|
---|---

# Step 4: Add OLED display
Remember Lab 3, Upload the code of Ex.3 there. build the circut and see if it works.
|Ex.2| Add this step to the report.|
---|---

# Wi-Fi
## Introduction

The objective of this part is to explain how to get started using the WiFi functionalities of the ESP32, more precisely how to scan surrounding WiFi networks and how to connect to a specific WiFi network.
This part will also cover getting some parameters, such as the local IP of the ESP32 when connected to the WiFi network, and also its MAC address. We will also cover how to disconnect from the WiFi network.

> Your ESP32 has a wifi module. 
## Setup
> The `WiFi.h` library allows an Arduino board to connect to the internet. It can serve as either a server accepting incoming connections or a client making outgoing ones. The library supports WEP and WPA2 Personal encryption, but not WPA2 Enterprise. Also note, if the SSID is not broadcast, the shield cannot connect.

The first thing we are going to do is including the WiFi.h library
```C
#include <WiFi.h>
//setup OLED
void setup(){
  display.begin(); // Instead of Serial.begin(9600)
  display.setTTYMode(true); // This set the TTY mode
  
}
```

Next, we will declare two global variables to hold our WiFi network credentials, more precisely the network name (SSID) and the password. 

```C
const char* ssid = "yourNetworkName";
const char* password = "yourNetworkPassword";
```

## Scanning for WiFi
```C
void scanNetworks() {
 
  int numberOfNetworks = WiFi.scanNetworks();
 
  display.printf("Number of networks found: %d\n",numberOfNetworks);
  
 
  for (int i = 0; i < numberOfNetworks; i++) {
 
    display.print("Network name: ");
    display.println(WiFi.SSID(i));
 
    display.print("Signal strength: ");
    display.println(WiFi.RSSI(i));
 
    display.print("MAC address: ");
    display.println(WiFi.BSSIDstr(i));
 
    //display.print("Encryption type: ");
    //display.print(WiFi.encryptionType(i)); the type is  wifi_auth_mode_t
   
    display.println("-----------------------");
 
  }
}
```

## Connecting to a Network
```C
void connectToNetwork() {
  WiFi.begin(ssid, password);
 
  while (WiFi.status() != WL_CONNECTED) {
    delay(1000);
    display.println("Establishing connection to WiFi..");
  }
  
  display.println("Connected to network");
}
```
## Print WiFi information
```C
void printWiFiInfo() {
  display.println(WiFi.macAddress());
  display.println(WiFi.localIP());
}
```
## Disconnect
```C
void disconnect(){
  WiFi.disconnect(true);
  display.print("Disconnected! ip:");
  display.println(WiFi.localIP());
}
```

## [More info Json, HTTP Get, HTTP Post, HTTPS (optional)](more.md)

# Thingspeak platform

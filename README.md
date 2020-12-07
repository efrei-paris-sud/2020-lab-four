# Introduction to Cloud
To communicate with other devices we need to use a board that has WiFi, Bluetooth or etc. 
Therefore we will use ESP 32 board which is suitable for this usage.

We want to provide a IoT dashboard for our Green House!!!
![](https://www.senseye.io/hs-fs/hub/533282/file-3715982174-png/blog-files/dashboard-1.png?width=753&height=561&name=dashboard-1.png)

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


### **ThingSpeak Setup for ESP32**

ThingSpeak is a free web service which helps us in IoT based projects. By using ThingSpeak server, we can monitor our data over the internet using the API and channels provided by ThingSpeak. In this section I am explaining about  **how to send sensor data of ESP32 to ThingSpeak server**. For this you have to follow following steps:

1.  Firstly go to  [https://thingspeak.com/](https://thingspeak.com/)  and create an account and sign in to this server.

2.  After signing in you will find below window in which number of channels are listed in this go to  _New channel_.

![ ThingSpeak Setup for ESP32](https://iotdesignpro.com/sites/default/files/inline-images/ThingSpeak-Setup-for-ESP32.png)

3.  After clicking on  _New Channel_  you will find a window in which you have to enter some details about the channel, in this project we want to analyze temperature and Hall sensor value of ESP32 so you will require 2 fields. So enter the details as shown and save the channel.

![ Start New Channel on ThingSpeak for ESP32](https://iotdesignpro.com/sites/default/files/inline-images/Start-New-Channel-on-ThingSpeak-for-ESP32.png)

4.  After saving of channel you will find channel stats window showing details about your channels.

![ Graphical Representation on ThingSpeak for ESp32](https://iotdesignpro.com/sites/default/files/inline-images/Graphical-Representation-on-ThingSpeak-for-ESp32.png)

5.  Now go to API key menu which shows you  **Write API keys and Read API key**, Copy Write API key as you will required this API during programming of ESP32.

![ Getting API Key from ThingSpeak for ESp32](https://iotdesignpro.com/sites/default/files/inline-images/Getting-API-Key-from-ThingSpeak-for-ESp32.png)


### Install ThingSpeak Communication Library for Arduino
In the Arduino IDE, choose Sketch/Include Library/Manage Libraries. Click the ThingSpeak Library from the list, and click the Install button.
[![Example Video of the porject](https://img.youtube.com/vi/w73kv6x05NE/0.jpg)](https://www.youtube.com/watch?v=w73kv6x05NE)

### Setup Arduino Sketch

We have provided a few Arduino sketch [examples](https://github.com/mathworks/thingspeak-arduino/tree/master/examples/ESP32)  with the ThingSpeak library. They are designed to work right away with no changes. To make the examples work with your ThingSpeak channel, you will need to configure the _myChannelNumber_  and  _myWriteAPIKey_ variables.

### Send an Analog Voltage to ThingSpeak

The WriteSingleField Arduino sketch example reads an analog voltage from pin 0, and writes it to a channel on ThingSpeak every 20 seconds. Load the example in the Arduino IDE. Make sure to select the correct Arduino board and COM port. Then, upload the code to your Arduino.

### Sending Multiple Values to ThingSpeak

Since ThingSpeak supports up to 8 data fields, you might want to send more than one value to ThingSpeak. To send multiple value to ThingSpeak from an Arduino, you use _ThingSpeak.setField(#,value)_ for each value to send and then use _ThingSpeak.writeFields(myChannelNumber, myWriteAPIKey)_ to send everything to ThingSpeak. Use the WriteMultipleFields Arduino sketch example to send multiple pin voltages to ThingSpeak.

### **Read Value** 
- Please see the   `ReadField` example. It is Reading from a public channel and a private channel on ThingSpeak.

|Ex.3| Build circuit of [Step 6 in Lab1](https://github.com/efrei-paris-sud/2020-lab-one#step-6-read-an-analog-value). Send its data to thingspeak dashboard! Take a photo of the dashboard|
---|---


# Advanced Topic (for those want to change a value from dashboard)
create your own plugin to change the LED state.

![Pure CSS3 iOS switch checkbox. A switch allows the user to quickly… | by  Pedro M. S. Duarte | codeburst](https://miro.medium.com/max/403/1*PjHEsBxh7B9BrYARqHcujQ.jpeg)

Go to thingspeak dashboard-> App-> plugin -> new

## put these codes into the boxes
> don't forget to change ,`led_field`, `readapikey` and `write api key` in the js

#### HTML
```html<html>
<head>

  %%PLUGIN_CSS%%
  %%PLUGIN_JAVASCRIPT%%
  
</head>
<body>
  
   <div class="pretty p-switch">
        <input type="checkbox" id="myCheck" onclick="myFunction()">
        <div class="state">
            <label>LED 1</label>
        </div>
    </div>

</body>
</html>

```
#### CSS
```css
<style type="text/css">
  body { background-color: #fff; }
</style>
<link rel="stylesheet" href="https://cdn.jsdelivr.net/npm/pretty-checkbox@3.0/dist/pretty-checkbox.min.css" />
    

```
#### JS
```js
<script type='text/javascript' src='https://ajax.googleapis.com/ajax/libs/jquery/1.9.1/jquery.min.js'></script>


<script type="text/javascript">
  // set your channel id here
  var channel_id = 1250634;
  var read_api_key = 'L7K0BVNWS2FFV3UG';
  var write_api_key = 'PPXSCLNQXIOJ2HCR';
  
  temp_field=1
  led_field=2
   
  function myFunction() {
  // Get the checkbox
    var checkBoxVal = $("#myCheck").is(":checked")
  // Write On channel
    $.getJSON('https://api.thingspeak.com/update?api_key=' + write_api_key+"&field"+led_field+"="+(checkBoxVal?1:0), function(data) {
      if(data==0){
        alert("fast toggle");
        return;
      }
      
    });


  }
  
  function updateStat(){
  $.getJSON('https://api.thingspeak.com/channels/' + channel_id + '/feed/last.json?api_key=' + read_api_key, function(data) {
      // get the data point
      p = data['field'+led_field];
      // if there is a data point display it
      if (p) {
        $("#myCheck").prop('checked',p==1);
      }
    });
  }
  
  $( document ).ready(function(){
  setInterval(updateStat,1000);

  });
</script>
```
### Add the plugin to your dashboard. is it working?

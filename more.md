
# Json 
Json is an open-standard file format that uses human-readable text to transmit data objects consisting of attribute–value pairs and array data types (or any other serializable value). It is a very common data format, with a diverse range of applications. [More info](https://en.wikipedia.org/wiki/JSON)

To use json parser in Arduino you should search for  ArduinoJSON v6. [More info](https://arduinojson.org/v6/doc/)
## Write Json
```C
  DynamicJsonDocument  doc(200);
  doc["sensor"] = "gps";
  doc["time"] = 1351824120;
  JsonArray data = doc.createNestedArray("data");
  data.add(48.756080);
  data.add(2.302038);
  String output;
  serializeJsonPretty(doc, output);// You can also use serializeJson(doc, Serial);
  display.println(output);
  // The above line prints:
  // {
  //   "sensor": "gps",
  //   "time": 1351824120,
  //   "data": [
  //     48.756080,
  //     2.302038
  //   ]
  // }
```
## Read Json
```C
  DynamicJsonDocument doc(1024);
  deserializeJson(doc, input);
  JsonObject obj = doc.as<JsonObject>();
  long time = obj["time"];
  display.println(time);
  int firstdata = obj["data"][0]; 
  display.println(firstdata);
```

# Http Request
The objective of this part is to explain how to send HTTP requests
First, You need to include this header.
```C
#include <HTTPClient.h>
```

## HTTP Get
To send a HTTP Get request, you can use the following code. [More Info](https://techtutorialsx.com/2017/05/19/esp32-http-get-requests/)
```C
void SendGetRequest()
 if ((WiFi.status() == WL_CONNECTED)) { //Check the current connection status
 
    HTTPClient http;
 
    http.begin("https://jsonplaceholder.typicode.com/posts/1"); //Specify the URL 
    //(A test json server: https://jsonplaceholder.typicode.com/guide.html)
    
    int httpCode = http.GET();     //Make the request
 
    if (httpCode > 0) { //Check for the returning code
 
        String payload = http.getString();
        display.println(httpCode);
        display.println(payload);
        
        DynamicJsonDocument doc(1024);
        deserializeJson(doc, payload);
        JsonObject obj = doc.as<JsonObject>();
        int userId = obj["userId"];
        display.println(userId);
      }
 
    else {
      display.println("Error on HTTP request Code:"+httpCode);
    }
 
    http.end(); //Free the resources
  }
```

## HTTP Post
[More Info](https://techtutorialsx.com/2017/05/20/esp32-http-post-requests/)
```C
void SendPostRequest()
  if ((WiFi.status() == WL_CONNECTED)) { //Check the current connection status
 
    http.begin("http://jsonplaceholder.typicode.com/posts"); //Specify destination for HTTP request
    http.addHeader("Content-Type", "application/json");
    
    DynamicJsonDocument  doc(200);
    doc["title"] = "title";
    doc["body"] = "body";
    doc["userId"] = "1";
    
    String output;
    serializeJsonPretty(doc, output);// You can also use serializeJson(doc, Serial);
    display.println(output);
    
    int httpCode = http.POST(output); //Send the actual POST request
    if(httpCode>0){
      String response = http.getString(); //Get the response to the request
      display.println(httpCode);   //Print return code
      display.println(response);           //Print request answer
    }else{
      display.print("Error on sending POST: ");
      display.println(httpCode);
    }
  }
}
```

## HTTPS (Optional)
Please Follow the instruction [here](https://techtutorialsx.com/2017/11/18/esp32-arduino-https-get-request/)



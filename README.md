aWOT
==========

Collection of tools to build RESTful stuff with Arduino compatible boards. Only some of the features work on Arduino Uno. There is just not enough ROM and RAM.

It's a hack and under constant change. Prepare to get your hands dirty.

## WoT?

Web Of Things, a sidekick of the IoT, is a concept where the current web development patterns and tools are recycled to integrate everyday objects to the existing World Wide Web.

In WoT the idea is to realize the IoT concept using the current web standards and practices. In order to do so WoT uses the REST architectural style implemented on top of the HTTP already in use all over the Internet.

The WoT concept can be taken a step further to allow the communications over other protocol stacks than the traditional TCP. A device designed to be controlled over Ethernet can be for example be managed over any number of compliant media such as Bluetooth, Wi-Fi and serial port communications.

## Documentation
http://www.awot.net

## Example

```cpp
#include <SPI.h>
#include <Ethernet.h>
#include <aWOT.h>

byte mac[] = { 0xDE, 0xAD, 0xBE, 0xEF, 0xFE, 0xED };
EthernetServer server(80);

WebApp app;

// define a handler function
void indexCmd(Request &req, Response &res) {

  // P macro for printing strings from program memory
  P(index) =
    "<html>\n"
    "<head>\n"
    "<title>Hello World!</title>\n"
    "</head>\n"
    "<body>\n"
    "<h1>Greetings middle earth!</h1>\n"
    "</body>\n"
    "</html>";

  res.success("text/html");
  res.printP(index);

}

void setup() {

  Serial.begin(115200);

  if (Ethernet.begin(mac)) {
    Serial.println(Ethernet.localIP());
  } else{
    Serial.println("Ethernet failed");
  }

  // mount the handler to the default router
  app.get("", &indexCmd);

}

void loop(){
  EthernetClient client = server.available();
  if (client){
      app.process(&client);
  }
}
```

## Licence

Copyright (c) 2014, Lasse Lukkari 

Based on 

Webduino,
Copyright 2009-2014 Ben Combee, Ran Talbott, Christopher Lee, Martin Lormes
Francisco M Cuenca-Acuna

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction, including without limitation the rights
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software, and to permit persons to whom the Software is
furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in
all copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN
THE SOFTWARE.

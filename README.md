MQTT example
============

This example walks you through the basic complete usage of Paho MQTT.

Introduction
------------

The MQTT library used is the C library developed by [Eclipse Paho](https://eclipse.org/paho/) (is an OpenSource project). You can find more information here:

* [Paho C Client](https://eclipse.org/paho/clients/c/).
* Git repository with the code: http://git.eclipse.org/c/paho/org.eclipse.paho.mqtt.c.git

MQTT C Client
-------------

The Paho MQTT C Client offers two APIs, one synchronous and one asynchronous. For any professional usage, you should target the asynchronous one since you don't want to stall any thread waiting or receiving messages. Thus, the examples given only offer the Asynchronous Calls.

### Dependencies and binaries

I have build the MQTT Paho source code into a static library called `libMQTT.a` with three headers: `MQTTAsync.h`, `MQTTClient.h`, `MQTTClientPers istance.h`. It can be found under Paho > external > MQTT.

The binary supports all iOS devices (ARMv7, ARMv7s, ARMv64, and i386 simulator) and OSX machines (x86_64).

The `libMQTT.a` static library needs the OpenSSL static library. I have compiled the latest OpenSSL library (1.0.2) into the file you can find in Paho > external > MQTT.

I decided to make two separate static libraries instead of one binary blob, for three reasons:

1. It is easy to update each library separately.
2. Right now each static library is too big (since it supports all those CPU architectures), you can reduce its size greatly by just releasing the static library for platform you are targeting. For example, in your distribution release for the app store, you can take the Simulator and x86_64 code from the binaries reducing the size of both MQTT and OpenSSL by 40% of the size (8.2 MB of the total size).
3. Once you have set up the XCode project for static libraries it doesn't really matter whether you add one or two. Having the benefit of using one or the other independently (if you need that).

### Adding static libraries to XCode

You need to do three steps:

1. Go to your project > `Build Settings` and in the search box look for `Header Search Path`. In the value of that plist array you should include the location of the folder where you have the external library. In my example: `$(PROJECT_DIR)/external`.
2. Go to your project > `Build Settings` and in the search box look for `Library Search Path`. In the value of that plist array you should also include the location of the folder where you have the external library (`$(PROJECT_DIR)/external` as before), but be sure to change the property to `recursive` (by default is `non-recursive`).
3. Go to your project > `Build Phases` and in the `Link Binary With Libraries`section drag and drop the static library binaries. In this case: `libcrypto.a`, `libssl.a`, `libMQTT.a`.

### Using MQTT

To use the MQTT code add the following headers to the files targeting MQTT communications.

```c
#import <MQTT/MQTTClient.h>
#import <MQTT/MQTTAsync.h>
```

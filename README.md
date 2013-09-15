FridgeTemp
===============================

Coming across a functioning minifridge out for disposal on the loading dock the other day was all the prompting I needed to finally get around to building that homebrew kegging setup I've wanted so long. But brewing is a precise art - the physical variables of the system are critical. How best to measure them but with an Arduino?!

What's Included:
----------------
The homebrewerator uses an Arduino Uno to read two (or as many as you like...) Maxim/Dallas DS18B20 1-Wire temperature sensors. In my setup, one of the two is taped to the side of the keg in the fridge, under some pipe insulation to accurately reflect the liquid temperature and not the ambient air. The other is left free hanging a few inches from any surface in the middle of the fridge, to measure ambient air temperature. These two temperatures are displayed on a 16x2 character LCD as follows:

```
Keg:    Ambient:
43.59   42.35
```

Ethernet:
---------
Check out the current kegerator temps at ~~http://keg.beerchallenged.com/~~! Long since out of operation. Sorry! This library doesn't really hold up that well to many queries, anyway.

Displaying temperatures locally is cool (and useful), but what happens when you turn the temperature down a little, only to come home from work and find frozen beer in your lines? No good! At least while I got everything diald in just right, I decided ethernet connectivity was necessary to check my temps from my phone on the go.

I used an ethernet shield I happend to have lying around, a Seeedstudio Ethernet Shield v1.1 - one of the pre-wiznet shields that only has a MAC/PHY chip (the ENC28J60). This means a few things for the code.

First, the normal ethernet library won't do. I used the library explained and downloadable from this page: http://www.nuelectronics.com/estore/index.php?main_page=project_eth. Worth noting is that the library found there no longer works with Arduino 1.0: I have updated a few files to make it compatible here: https://github.com/alexwhittemore/EtherShield28j60.

Second, since there's not a proper TCP/IP stack, all the data you want to convey must fit within one packet. It's not the worst constraint in the world for light duty data-to-web applications like this, but it's something to consider.

DS18B20/OneWire:
----------------
You also need to have the OneWire library. You can get that over here: http://www.pjrc.com/teensy/td_libs_OneWire.html

Additionally, I make use of the DallasTemperature library from Miles Burton, which consolidates lots of other working DS18B20 examples from others. That's here: https://github.com/milesburton/Arduino-Temperature-Control-Library NOTE: if you `git clone` this to your library directory, you'll need to change the folder name to "DallasTemperature".

To Do:
------
There are a bunch of things this doesn't do which would be nice

* Control - it's easy enough to use an arduino to control an external relay. In fact, this kegerator became a class project in embedded linux, and that hardware setup supported this. This iteration is quite old, though.
* Local interaction - Going along the lines of control, we should be able to change the setpoint locally, at the fridge.
* Web interaction - change the setpoint from a web interface.

Going Forward:
--------------
It's been a LONG time since I did this project the first time. I'm only even revisiting it now because I just kegged some homebrew for the first time in a while and I need to watch the temperatures. 

It seems the most obvious thing to do is to replace the ethernet component with something a bit better. The ENC28J60 is super cheap and lovely for it, but doing much at all with the web interface is VERY hard as a result. It's really only useful for implementing some kind of basic get-the-data-out interface. Using an Arduino-official ethernet shield is an obvious solution, but I've also got a Netduino Plus lying around, and that kills two birds with one stone.

The screen is also a bit meh, small touch LCDs are cheap these days, and I have one from Seeed. That'd neatly take care of display and local control.

Of course, from a cost perspective it's hard to beat a raspberry pi, and a $10 wifi dongle would be a nice bonus, so I'll probably just get lazy and do that.

Given the limitations of the current system, and given that this sketch is already within ~5 bytes of the maximum program size for an ATMega168, I suspect I'll abandon arduino going forward.

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
Check out the current kegerator temps at keg.beerchallenged.com!

Displaying temperatures locally is cool (and useful), but what happens when you turn the temperature down a little, only to come home from work and find frozen beer in your lines? No good! At least while I got everything diald in just right, I decided ethernet connectivity was necessary to check my temps from my phone on the go.

I used an ethernet shield I happend to have lying around, a Seeedstudio Ethernet Shield v1.1 - one of the pre-wiznet shields that only has a MAC/PHY chip (the ENC28J60). This means a few things for the code.

First, the normal ethernet library won't do. I used the library explained and downloadable from this page: http://www.nuelectronics.com/estore/index.php?main_page=project_eth. Worth noting is that the library found there no longer works with Arduino 1.0: I have updated a few files to make it compatible here: https://github.com/alexwhittemore/EtherShield28j60.

Second, since there's not a proper TCP/IP stack, all the data you want to convey must fit within one packet. It's not the worst constraint in the world for light duty data-to-web applications like this, but it's something to consider.


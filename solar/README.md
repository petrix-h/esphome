# TODO Document better

- 5V 1W solar panel -> TP4056 -> 18650 cell (protected) 
- 18650 cell -> 1702 -> ESP32-S2
- ADS1115 over I2C

TODO: 
- add temperature sensor for solarpanel
- add temperature sensor for 18650 cell
- consider a i2c temp/hum sensor if all is in a enclosure
- consider a water sensor (figure out how) with 2 wires at the bottom of enclosure and GPIO enabled checks to stop it from trying to become an electrolyser and empty the battery
- figure out how to do OTA's properly with HA toggle button thing
- consider adding a touch sensor to wake it up
- consider adding an oled or something to view things when touch sensor is used
- figure out how the franklin lightning sensor interupt works, can it wake up the ESP through a pin (should be able to)
- check the inspiration videos for adding a soil sensor
- figure out if sensor power can be toggled from GPIO (directly if low enugh current) and still have things working over I2C
  - consider adding a luminosity sensor
  - consider adding a pressure sensor
 
Cosider making a "I2C rail": 
- PCB with connected lines, 4 pins:
  - power
  - SCA
  - SCL
  - Ground
- using female connectors, could just "plug and pray" sensors in a line... think of spacing requirements... could vertical spacing also be done? also could have 2 180 degree rails if:
  - Ground
  - SCL
  - SCA
  - 3.3v
  - SCA
  - SCL
  - Ground 

## Modifications
TP4056 default charge resistor replaced with 10k resistor to limit charging to 100mA instead of 1A
TP4056 PROG pin can also be used to read the charge rate, formula in datasheet (or example yaml)

ADS1115
- A0 = Battery voltage through voltage divider 56k / 100k (10k to ground).. in code use multiplier 1.56 (156/100)
- A1 = Solar Panel Voltage through voltage divider 100k/100k... in code use multiplier 2 (200/100)
- A2 = Charge current, TP4056 gives charge current in mV (max 1V @ 1A) so no voltage divider needed direct connection to PROG pin

1702 3.3v LDO regulator
- using 2.2 uF tantalum caps instaead of 1uF, seems to work fine..
- possibly needing a large cap (2200uF tested) on regulated side if/when ESP starts to have more sensors, otherwise occationally bootloops.. Could also be the battery (not the best)

### Inspirations
- https://www.youtube.com/channel/UC7QFnHZ-c0i2T5F4BJ8vwsA
- https://youtu.be/37kGva3NW8w?feature=shared
- https://youtu.be/f2yMs-JAyQM?feature=shared

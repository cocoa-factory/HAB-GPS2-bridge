This application is a component of HAB, the high-altitude balloon flight controller.  It provides a bridge between to incoming serial data from the backup GPS (an eTrex Legend) and the main flight computer's I2C bus.  Rather than use a larger MCU to provide more on-board USARTs, we chose a bridge.  Further the secondary control off-loads the NMEA parsing from the primary controller.

---

## Target MCU ##
This code has been tested on an ATmega 328 AVR; but the code is compact enough to work with controllers with less flash memory - such as an ATmega 48 or ATmega 88.  The optimized code is just under 4K; so an ATmega 48 may work acceptably.

To reduce the USART error, the clock frequency should be:

* 1.8432 MHz
* 3.6864 MHz
* 7.3728 MHz
* 14.7456 MHz
* 16.5888 MHz

---

## Project files ##

### settings.h/.c ###
The settings files provide persistent settings functionality for the application.  Additional settings can be added by modifying the `settings_record_t` `struct`.  


### gps.h/.c ###
The gps file mainly takes care of parsing the NMEA strings from the GPS.  Currently, it provides a limited amount of data.

To access coordinates components (lat/long) use the `gps_data` structure like:

    gps_data.latitude.degrees

or

    gps_data.latitude.minutes

### TWI_slave.h/.c ###
The interface to the TWI/I2C bus.  This code is provided by Atmel.

---

## Operation ##

This device responds to the following operation codes:

#### VEL_KTS  0x20 ####
Returns the velocity in knots as a single byte.

#### LAT 0x40 ####
Returns the current latitude as four bytes: degrees, minutes, seconds, and direction.  
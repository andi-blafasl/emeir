# emeir
Monitor the counter of an electricity meter with Arduino and infrared light sensor.

![Electricity meter with infrared light barrier](https://www.kompf.de/tech/images/countemeir.jpg)


The software consists of two parts:

* Data acquisition part running on an Arduino Nano. It controls the infrared light barrier, detects trigger levels and communicates with a master computer over USB serial.
* Data recording part running on a the master computer (Raspberry Pi). It retrieves the data from the Arduino over USB serial and stores counter and consumption values into a round robin database.

There is a blog in german language that explains use case and function: [Infrarot Lichtschranke mit Arduino zum Auslesen des Stromzählers](https://www.kompf.de/tech/emeir.html).


## Commands from host (RasPi) to Arduino

* __D__ - retrieve and print raw data
* __T__ - enter trigger mode and print trigger data (0/1)
* __S__ _low_ _high_ - Set trigger levels (e.g. 85 90)
* __C__ - Cancel data acquisition and enter command mode

Arduino is in trigger mode upon start - Send __C__ to enter command mode

## Schematics

![Schematics](https://www.kompf.de/tech/images/reflsensor.png)

## Sync with meter

A new database starts with 0 in the countervalue. To sync the database with the meter, just note the value (e.g 56122.0) and use following command to update the RRD-File.

    rrdtool update emeir.rrd N:56122.0:0

After the Update, just restart wairc.py. It will read the saved meter value from the Database and continue counting.


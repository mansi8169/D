**Oscilloscope:**

\#pins: VDD-1 , GND-6 , SDA-3 , SCL-5

sudo raspi-config

pip install board --break-system-packages

sudo pip install drawnow --break-system-packages

sudo apt-get install -y i2c-tools python3-smbus

python3 -m pip install --upgrade --no-cache-dir adafruit-blinka adafruit-circuitpython-busdevice adafruit-circuitpython-ads1x15 --break-system-packages

fingerprint:
1: VCC Red Connect it to USB-
TTL(5V) : VCC
2: GND Black Connect it to USB-TTL:GND
3: Tx Yellow Connect it to USB-TTL:Rx
4: Rx White Connect it to USB-TTL:

Tx
Enter the following commands in terminal.
 sudo raspi-config
 Go to Interfacing options -&gt;I2C &amp; SPI -&gt; Enable it.
 pip install pyfingerprint –break-system-packages
 ls /dev/ttyUSB*
If required enter the following commands
 sudo apt-get -f install
 sudo usermod -a -G dialout pi
If any module error after doing these steps so do the below steps:
Install the module
Sudo pip3 install pyfingerprint



**7Segment:**

GND -14 GND

VCC - 4 5V 

DI0 - 18 GPIO 24

CLK- 16 GPIO 23

pip install RPi.GPIO --break-system-packages



import sys

import time

import datetime

import RPi.GPIO as GPIO

import tml1637

GPIO.setmode(GPIO.BCM) # or GPIO.BOARD depending on your wiring

Display = tml1637.TM1637()

Display.Clear()

Display.SetBrightness(1)

while True:

&nbsp;	now = datetime.datetime.now()

&nbsp;	hour = now.hour

&nbsp;	minute = now.minute

&nbsp;	second = now.second

&nbsp;	currenttime = \[int(hour / 10), hour % 10, int(minute / 10), minute % 10]

&nbsp;	Display.Show(currenttime)

&nbsp;	Display.ShowDoublepoint(second % 2)

&nbsp;	time.sleep(1)





**GPS:**

\#Hardware Wiring — GPS to Pi GPIO

\# GPS Module -> Raspberry Pi GPIO

\# VCC        -> Pin 1 (3.3V Power)

\# GND        -> Pin 6 (Ground)

\# TX         -> Pin 10 (GPIO 15 - RXD)

\# RX         -> Pin 8 (GPIO 14 - TXD)

sudo apt install python3-venv

python3 -m venv myenv

source myenv/bin/activate

\#configuration(diff terminal)

dtparam=spi=on

dtoverlay=pi3-disable-bt

core\_freq=250

enable\_uart=1

force\_turbo=1

sudo systemctl stop serial-getty@ttyS0.service

sudo systemctl disable serial-getty@ttyS0.service

sudo systemctl enable serial-getty@ttyAMA0.service

sudo apt-get install minicom

pip install pynmea2 --break-system-packages

sudo cat /dev/ttyS0

code:

import time

import serial

import string

import pynmea2

import RPi.GPIO as gpio #if error here use command:- pip install Rpi.GPIO

gpio.setmode(gpio.BCM)

port = "/dev/ttyS0" # the serial port to which the pi is connected.

\#create a serial object

ser = serial.Serial(port, baudrate = 9600, timeout = 0.5)

while 1:

&nbsp;	try:

&nbsp;		data = ser.readline()

&nbsp;		print(data)

&nbsp;	except:

&nbsp;		print("loading")

&nbsp;		#wait for the serial port to churn out data



&nbsp;	if data\[0:6] == \&#39;$GPRMC\&#39;:

&nbsp;		msg = pynmea2.parse(data)

&nbsp;		print msg

&nbsp;		time.sleep(2)







**7Segment LED**

GND - 14

VCC- 4

DI0 - 18

CLK - 16

pip install RPi.GPIO --break-system-packages





**RFID:**

5V Pin 4 (5V)

GND Pin 6 (GND)

SDA Pin 3 (GPIO 2)

SCL Pin 5 (GPIO 3)

sudo raspi-config (enable i1c)

sudo apt install libusb-dev libpcsclite-dev i2c-tools

cd ~

wget http://dl.bintray.com/nfc-tools/sources/libnfc-1.7.1.tar.bz2

tar -xf libnfc-1.7.1.tar.bz2

cd libnfc-1.7.1

./configure --prefix=/usr --sysconfdir=/etc

make

sudo make install

sudo mkdir /etc/nfc

sudo nano /etc/nfc/libnfc.conf

\#Paste the following content:

allow\_autoscan = true

allow\_intrusive\_scan = false

log\_level = 1

device.name = "\_PN532\_I2c"

device.connstring = "pn532\_i2c:/dev/i2c-1"

\#Save and exit (Ctrl + O, Enter, then Ctrl + X).

\#Hardware Wiring

Set PN532 RFID module to I2C mode using switch:

 SEL0 = HIGH, SEL1 = LOW

i2cdetect -y 1

nfc-list

nfc-poll

python3 rfid\_reader.py


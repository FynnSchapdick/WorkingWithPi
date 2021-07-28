# WorkingWithPi
Usefull things to know working with "pies" ;-)


## Enabling SSH before first boot
Place the following two files in your fresh created image

/boot/ssh -> needs to be empty
/boot/wpa_supplicant.conf -> contains your wireless info

For example:
ctrl_interface=DIR=/var/run/wpa_supplicant GROUP=netdev
update_config=1
country=US

network={
	ssid="your-network-service-set-identifier"
	psk="your-network-WPA/WPA2-security-passphrase"
	key_mgmt=WPA-PSK
}

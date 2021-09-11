# WorkingWithPi
Usefull things to know working with "pies" ;-)


## Enabling SSH before first boot
Place the following two files in your fresh created image:

- /boot/ssh -> needs to be empty

- /boot/wpa_supplicant.conf -> contains your wireless info

For example:

```
ctrl_interface=DIR=/var/run/wpa_supplicant GROUP=netdev
update_config=1
country=US

network={
	ssid="your-network-service-set-identifier"
	psk="your-network-WPA/WPA2-security-passphrase"
	key_mgmt=WPA-PSK
}
```

Default ssh credentials -> pi / raspberry

## Change Hostname

Check what's the current hostname
```
hostname
```

Set new hostname
```
sudo hostname -b {NEUER_NAME}
```

Check if hostname contains in any configurations
```
sudo grep -lr "{ALTER_NAME}" /etc/*
```

Important: After changing hostname, the hostname must not contain in the "/etc/hosts", so you have to change it by
```
sudo nano /etc/hosts
```

and you have to remove all ssh-keys for that hostname
```
sudo rm /etc/ssh/ssh_host_*
```

After that we have to reconfigure the ssh-server
```
sudo dpkg-reconfigure openssh-server
```

And then restart the service
```
sudo service ssh restart
```

## SSH-Keys

Generate a new ssh-key
```
ssh-keygen -t ed25519 -C "your_email@example.com"
```

Add ssh-key to agent
```
ssh-add ~/.ssh/id_ed25519
```

## Install Docker on Pi

```
curl -fsSL https://get.docker.com | sh
sudo usermod -aG docker pi
```

Reboot

```
sudo apt-get install -y libffi-dev libssl-dev
sudo apt-get install -y python3 python3-pip
sudo apt-get remove python-configparser
sudo pip3 -v install docker-compose
```

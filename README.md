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

## Install Kafka and Zookeeper

Create file docker-compose.yml

For example:

```
version: '2'
services:
  zookeeper:
    image: confluentinc/cp-zookeeper:latest
    environment:
      ZOOKEEPER_CLIENT_PORT: 2181
      ZOOKEEPER_TICK_TIME: 2000
    ports:
      - 22181:2181
  
  kafka:
    image: confluentinc/cp-kafka:latest
    depends_on:
      - zookeeper
    ports:
      - 29092:29092
    environment:
      KAFKA_BROKER_ID: 1
      KAFKA_ZOOKEEPER_CONNECT: zookeeper:2181
      KAFKA_ADVERTISED_LISTENERS: PLAINTEXT://kafka:9092,PLAINTEXT_HOST://localhost:29092
      KAFKA_LISTENER_SECURITY_PROTOCOL_MAP: PLAINTEXT:PLAINTEXT,PLAINTEXT_HOST:PLAINTEXT
      KAFKA_INTER_BROKER_LISTENER_NAME: PLAINTEXT
      KAFKA_OFFSETS_TOPIC_REPLICATION_FACTOR: 1
```

Start Kafka

```
docker-compose up -d
```

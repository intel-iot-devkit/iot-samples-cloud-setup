# C++ MQTT setup

## Updating the MQTT C client library on the Edison
In order to use MQTT with the "How To Code" samples, you will need to update the Paho MQTT C client libs on the Edison. To do this, SSH into the Edison, and then run the following commands from the Edison itself:

```
opkg install coreutils
opkg remove paho-mqtt-dev
opkg remove paho-mqtt --force-depends
git clone https://github.com/eclipse/paho.mqtt.c.git
cd paho.mqtt.c
export prefix=/usr; make install
```

Then logout from the SSH session, and copy the include files and compiled libs back from the Edison to the cross compiler on your local machine like this:
```
scp -r USERNAME@xxx.xxx.x.xxx:/usr/include/MQTT* ~/Downloads/iotdk-ide-linux/devkit-x86/sysroots/i586-poky-linux/usr/include
scp USERNAME@xxx.xxx.x.xxx:/usr/lib/libpaho-mqtt* ~/Downloads/iotdk-ide-linux/devkit-x86/sysroots/i586-poky-linux/usr/lib
```

Note that you must change `USERNAME@xxx.xxx.x.xxx` to match whatever username and IP address you set your board to. ALso, change `~/Downloads/iotdk-ide-linux` to match the location on your computer where you installed the Intel® IoT Developer Kit.

## Running code on Edison
When running the code on the Edison, you need to set the following "Commands to execute before application":

```
chmod 755 /tmp/alarm-clock; export MQTT_SERVER="ssl://A1QBI9OBPG6VY7.iot.us-west-2.amazonaws.com:8883"; export MQTT_CLIENTID="edison1"; export MQTT_CERT="/home/root/.ssh/cert.pem"; export MQTT_KEY="/home/root/.ssh/privateKey.pem"; export MQTT_CA="/etc/ssl/certs/VeriSign_Class_3_Public_Primary_Certification_Authority_-_G5.pem"; export MQTT_TOPIC="devices/edison1"
```

Click on the "Apply" button to save these settings.

Click on the "Run" button to run the code on the Edison.

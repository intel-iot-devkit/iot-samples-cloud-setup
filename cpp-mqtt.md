# C++ MQTT\* setup

## Updating the MQTT\* C client library on the Intel® Edison board

To use MQTT\* with the how-to code examples, you need to update the Paho\* MQTT\* C client libraries on the Intel® Edison board. To do this:

1. Establish an SSH connection to your Intel® Edison board, and then run the following commands from the board:

        opkg install coreutils
        opkg remove paho-mqtt-dev
        opkg remove paho-mqtt --force-depends
        git clone https://github.com/eclipse/paho.mqtt.c.git
        cd paho.mqtt.c
        export prefix=/usr; make install

2. Log out from the SSH session and connect to the Docker\* container as follows. First determine the container ID by listing all of the current containers using the `docker ps` command. You should see output like the following:

        CONTAINER ID        IMAGE                                    COMMAND             CREATED             STATUS              PORTS                   NAMES
56d3928f0a67        inteliotdevkit/intel-iot-wrs-64:latest   "bash"              5 days ago          Up 33 minutes       0.0.0.0:32771->22/tcp   intel-iot-wrs-64-workspace-iot1-160081000
4b91ecd3d1ad        inteliotdevkit/intel-iot-yocto:latest    "/bin/bash"         5 weeks ago         Up 2 days           0.0.0.0:32769->22/tcp   intel-iot-yocto-workspace-iot1-160081000
\

in this case, let's assume we want to connect to the `/home/ron/Development/intel/intel-iot-examples-mqtt/cpp-mqtt.md` container. The **Container ID** for this container is `56d3928f0a67`. Attach to it as follows:

        docker exec -t -i 56d3928f0a67 /bin/bash

This should bring up the bash prompt on the running container. Now you should be able to run the following commands:

        git clone https://github.com/eclipse/paho.mqtt.c.git
        cd paho.mqtt.c
        export prefix=/usr; make install

Once this command has completed with the installation, you can exit the container by running the `exit` command.

## Updating the MQTT\* C client library on the Intel® IoT Gateway.

To use MQTT\* with the how-to code examples, you need to update the Paho\* MQTT\* C client libraries on the Intel® IoT Gateway. To do this:

1. Establish an SSH connection to your Intel® IoT Gateway, and then run the following commands from the gateway:

        smart install openssl-dev
        git clone https://github.com/eclipse/paho.mqtt.c.git
        cd paho.mqtt.c
        export prefix=/usr; make install

2. Log out from the SSH session and copy the include files and compiled libraries back from your Intel® IoT Gateway to the cross compiler on your local machine as follows:

        scp -r USERNAME@xxx.xxx.x.xxx:/usr/include/MQTT* ~/Downloads/iotdk-ide-linux/devkit-x86/sysroots/i586-poky-linux/usr/include
        scp USERNAME@xxx.xxx.x.xxx:/usr/lib/libpaho-mqtt* ~/Downloads/iotdk-ide-linux/devkit-x86/sysroots/i586-poky-linux/usr/lib

where `USERNAME@xxx.xxx.x.xxx` is the username and IP address you set your board to and `~/Downloads/iotdk-ide-linux` is the location on your computer where you installed the Intel® IoT Developer Kit.

## Running code on the Intel® Edison board

1. In the **Commands to execute before application** field of your IDE, type the following:

        chmod 755 /tmp/alarm-clock; export MQTT_SERVER="ssl://A1QBI9OBPG6VY7.iot.us-west-2.amazonaws.com:8883"; export MQTT_CLIENTID="edison1"; export MQTT_CERT="/home/root/.ssh/cert.pem"; export MQTT_KEY="/home/root/.ssh/privateKey.pem"; export MQTT_CA="/etc/ssl/certs/VeriSign_Class_3_Public_Primary_Certification_Authority_-_G5.pem"; export MQTT_TOPIC="devices/edison1"

2. Click the **Apply** button to save these settings.
3. Click the **Run** button to run the code on the Intel® Edison board.

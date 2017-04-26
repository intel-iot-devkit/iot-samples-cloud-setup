# C++ additional setup

## Updating the MQTT\* C client library

### Updating the MQTT\* C client library on the Intel® Edison board

To use MQTT\* with the how-to code examples, you need to update the Paho\* MQTT\* C client libraries on the Intel® Edison board. To do this:

1. Establish an SSH connection to your Intel® Edison board, and then run the following commands from the board:

        opkg install coreutils
        opkg remove paho-mqtt-dev
        opkg remove paho-mqtt --force-depends
        git clone https://github.com/eclipse/paho.mqtt.c.git
        cd paho.mqtt.c
        export prefix=/usr; make install

2. Log out from the SSH session using the `exit` command.

### Updating the MQTT\* C client library on the Intel® IoT Gateway.

To use MQTT\* with the how-to code examples, you need to update the Paho\* MQTT\* C client libraries on the Intel® IoT Gateway. To do this:

1. Establish an SSH connection to your Intel® IoT Gateway, and then run the following commands from the gateway:

        smart install openssl-dev
        git clone https://github.com/eclipse/paho.mqtt.c.git
        cd paho.mqtt.c
        export prefix=/usr; make install

2. Log out from the SSH session using the `exit` command.

### Updating the MQTT\* C client library in Intel® System Studio.

1. First, connect to the Docker\* container that corresponds to the target platform you would like to build for. Determine the container ID by listing all of the current containers using the `docker ps` command. You should see output like the following:

```
CONTAINER ID        IMAGE                                    COMMAND             CREATED             STATUS              PORTS                   NAMES
56d3928f0a67        inteliotdevkit/intel-iot-wrs-64:latest   "bash"              5 days ago          Up 33 minutes       0.0.0.0:32771->22/tcp   intel-iot-wrs-64-workspace-iot1-160081000
4b91ecd3d1ad        inteliotdevkit/intel-iot-yocto:latest    "/bin/bash"         5 weeks ago         Up 2 days           0.0.0.0:32769->22/tcp   intel-iot-yocto-workspace-iot1-160081000
\
```

in this case, let's assume we want to connect to the `inteliotdevkit/intel-iot-wrs-64:latest` container. The **Container ID** for this container is `56d3928f0a67`. Attach to it as follows:

        docker exec -t -i 56d3928f0a67 /bin/bash

This should bring up the bash prompt on the running container.

2. Now you should be able to run the following commands within the container:

        git clone https://github.com/eclipse/paho.mqtt.c.git
        cd paho.mqtt.c
        export prefix=/usr; make install

3. Once these commands have completed with the installation, you can exit the container by running the `exit` command.

## Updating the Websocketpp\* library

### Updating the Websocketpp\* library on the Intel® Edison board

1. Establish an SSH connection to your Intel® Edison board, and then run the following commands from the board:

        cd ~
        git clone https://github.com/zaphoyd/websocketpp.git
        cd websocketpp
        cmake .
        make install

### Updating the Websocketpp\* library on the Intel® IoT Gateway.

1. First, check to see if you have CMake\* installed:

        cmake --version

If CMake is already installed, you can skip to the next step.

If you receive an error message, it means you need to install CMake. Run the following commands:

        cd ~
        wget https://cmake.org/files/v2.8/cmake-2.8.12.tar.gz
        tar -zxvf cmake-2.8.12.tar.gz
        cd cmake-2.8.12
        ./bootstrap
        make
        make install

3. Now, install Websocketpp\* by running the following commands:

        cd ~
        git clone https://github.com/zaphoyd/websocketpp.git
        cd websocketpp
        cmake .
        make install

### Updating the Websocketpp\* library in Intel® System Studio.

1. First, connect to the Docker\* container that corresponds to the target platform you would like to build for. Determine the container ID by listing all of the current containers using the `docker ps` command. You should see output like the following:

```
CONTAINER ID        IMAGE                                    COMMAND             CREATED             STATUS              PORTS                   NAMES
56d3928f0a67        inteliotdevkit/intel-iot-wrs-64:latest   "bash"              5 days ago          Up 33 minutes       0.0.0.0:32771->22/tcp   intel-iot-wrs-64-workspace-iot1-160081000
4b91ecd3d1ad        inteliotdevkit/intel-iot-yocto:latest    "/bin/bash"         5 weeks ago         Up 2 days           0.0.0.0:32769->22/tcp   intel-iot-yocto-workspace-iot1-160081000
\
```

in this case, let's assume we want to connect to the `inteliotdevkit/intel-iot-wrs-64:latest` container. The **Container ID** for this container is `56d3928f0a67`. Attach to it as follows:

        docker exec -t -i 56d3928f0a67 /bin/bash

This should bring up the bash prompt so you can run commands within the container.

2. First, check to see if you have CMake\* installed:

        cmake --version

If CMake is already installed, you can skip to the next step.

If you receive an error message, it means you need to install CMake. Run the following commands:

        cd ~
        wget https://cmake.org/files/v2.8/cmake-2.8.12.tar.gz
        tar -zxvf cmake-2.8.12.tar.gz
        cd cmake-2.8.12
        ./bootstrap
        make
        make install

3. Now, install Websocketpp\* by running the following commands:

        cd ~
        git clone https://github.com/zaphoyd/websocketpp.git
        cd websocketpp
        cmake .
        make install

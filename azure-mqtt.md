# Connecting to Microsoft\* Azure\* IoT Hub using MQTT\*

## Microsoft\* Azure\* initial setup

1. Create an account on [https://azure.microsoft.com/en-us](https://azure.microsoft.com/en-us), if you do not yet have one.
![](https://github.com/hybridgroup/intel-iot-examples-mqtt/blob/master/images/azure/create-free-account.png)

2. Log into your account.
![](https://github.com/hybridgroup/intel-iot-examples-mqtt/blob/master/images/azure/sign-in-to-azure.png)

3. Click **New > Internet of Things > Azure IoT Hub**.
![](https://github.com/hybridgroup/intel-iot-examples-mqtt/blob/master/images/azure/create-new-iot-hub.png)

4. Enter the required information for your new Azure\* IoT Hub and click **Create**.
![](https://github.com/hybridgroup/intel-iot-examples-mqtt/blob/master/images/azure/create-new-iot-hub-2.png)

Your new Azure\* IoT Hub is created within a few moments.

## Download the **sastoken** command line program

In order to authenticate to the Azure\* IoT Hub, you need to create a Shared Access Signature (SAS) token. The easiest way to do this, is using a command-line program called **sastoken**.

Precompiled binaries for Windows\*, OS X\*, and Linux\* have been created and can be found for download here:

[https://github.com/intel-iot-devkit/intel-iot-examples-mqtt/releases](https://github.com/intel-iot-devkit/intel-iot-examples-mqtt/releases)

All you need to do is download the correct version for your platform, and put the file somewhere where you will be able to run it from the command-line.

You can find the source code for the `sastoken` program under `support/azure/sastoken.go`.

## Obtain a shared access signature (SAS) token for administrative use

Once your Azure\* IoT Hub is created, you need to obtain an SAS token to perform administrative actions, such as creating or listing devices. To do this:

1. On your dashboard, click the link to your new Azure\* IoT Hub.
2. Go to **Settings > Shared access policies > registryReadWrite**.
![](https://github.com/hybridgroup/intel-iot-examples-mqtt/blob/master/images/azure/obtain-sas.png)
3. Obtain the **Primary key** and the corresponding **Connection string**.
4. Create an SAS token by running the command as follows:

        sastoken <Your IoT Hub Name>.azure-devices.net/devices/ <Your primary key> 1440 registryReadWrite

**Note:** For this to work, you must be in the directory where you downloaded the **sastoken** program (e.g., `C:\Users\me\Downloads`).

Your SAS token should look similar to this:

![](https://github.com/hybridgroup/intel-iot-examples-mqtt/blob/master/images/azure/sas-example.png)

This token will only be valid for 24 hours (1440 minutes) and should be used only for performing administrative functions, such as creating new devices. Each device also needs its own SAS token, which we will create in subsequent steps.

## Create a new device

You can use the `curl` command to create a new device for your Azure\* IoT Hub using the SAS token with **registryReadWrite** access as obtained above. For example, to create a new device named "edison1", issue the following command:

```
$ curl -i -X PUT -H "Content-Type: application/json" -H "Authorization: <Your SharedAccessSignature>" -d "{deviceId: \"edison1\"}" https://<Your IoT Hub Name>.azure-devices.net/devices/edison1?api-version=2016-02-03
```

If you are using Windows\*, you may need to install Cygwin* to be able to use `curl`. See [https://github.com/hybridgroup/intel-iot-examples-mqtt/blob/feature/image-link/installing-cygwin.md](https://github.com/hybridgroup/intel-iot-examples-mqtt/blob/feature/image-link/installing-cygwin.md) for instructions.

You should receive a response that looks like this:

![](https://github.com/hybridgroup/intel-iot-examples-mqtt/blob/master/images/azure/create-new-device-curl.png)

## Get the list of devices

You can use `curl` to get the list of current devices for your Azure\* IoT Hub using the SAS token with **registryReadWrite** access as follows:

```
curl -i -H "Accept: application/json" -H "Authorization: <Your SharedAccessSignature>" https://<Your IoT Hub Name>.azure-devices.net/devices?api-version=2016-02-03
```

You should receive a response that looks like this:

![](https://github.com/hybridgroup/intel-iot-examples-mqtt/blob/master/images/azure/list-devices-curl.png)

You need the device's **Primary key** or **Secondary key** to obtain an SAS token for enabling the device to connect to the Azure\* IoT Hub.

## Obtain an SAS token for device use

You need to create an SAS token for enabling the Intel® Edison board to connect to your Azure\* IoT Hub for recording data. The SAS token for this purpose has fewer privileges than the one created for administrative use. You can use `sastoken` as follows:

```
sastoken <Your IoT Hub Name>.azure-devices.net/devices/<Your device name> <Your device primary key> 1440
```

Your SAS token should look similar to this:

![](https://github.com/hybridgroup/intel-iot-examples-mqtt/blob/master/images/azure/device-sas-example.png)

It is valid for 24 hours (1440 minutes) and if you wish to change the duration for how long the SAS token will be valid, enter a different number of minutes. Note that the SAS token for a specific device should be used only for the device for which it is created. In other words, each device that you wish to connect needs its own SAS token.

## Summary

If you have followed all the steps above, you should have all the information that your program needs to connect to the MQTT\* server:

- `MQTT_SERVER` - use `<Your IoT Hub Name>.azure-devices.net`, along with the `ssl://` (for C++) or the `mqtts://` (for JavaScript\*) protocol

- `MQTT_CLIENTID` - use `<Your device name>`

- `MQTT_TOPIC` - use `devices/<Your device name>/messages/events/`

- `MQTT_USERNAME` - use `<Your IoT Hub Name>.azure-devices.net/<Your device name>`

- `MQTT_PASSWORD` - use the string with your device's SAS token.

## Additional setup for C++

When running your C++ code on the Intel® Edison board, you need to set the MQTT\* client parameters in Eclipse\*. To do that:

1. Go to **Run configurations** and, in the **Commands to execute before application** field, type the following:

        chmod 755 /tmp/<Your app name>; export MQTT_SERVER="ssl://<Your IoT Hub Name>.azure-devices.net:8883"; export MQTT_CLIENTID="<Your device ID>"; export MQTT_USERNAME="<Your IoT Hub Name>.azure-devices.net/<Your device name>"; export MQTT_PASSWORD="<Your device SAS token>"; export MQTT_TOPIC="devices/<Your device name>/messages/events/"

2. Click the **Apply** button to save these settings.
3. Click the **Run** button to run the code on your board.

## Additional setup for JavaScript*

When running your JavaScript\* code on the Intel® Edison board, you need to set the MQTT\* client parameters in the Intel® XDK IDE. Add the following entries to the **config.json** file:

```
{
 "MQTT_SERVER": "mqtts://<Your IoT Hub Name>.azure-devices.net:8883",
 "MQTT_CLIENTID": "<Your device name>",
 "MQTT_USERNAME": "<Your IoT Hub Name>.azure-devices.net/<Your device name>",
 "MQTT_PASSWORD": "<Your device SAS token>",
 "MQTT_TOPIC": "devices/<Your device name>/messages/events/"
}
```

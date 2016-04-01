# Connecting to Microsoft Azure IoT Hub using MQTT

## MS Azure Initial Setup

- Create an account on Microsoft Azure, if you do not yet have one.
![](../../images/azure/create-free-account.png)

- Login to your Microsoft Azure account.
![](../..//images/azure/sign-in-to-azure.png)

- Create a new IoT Hub by clicking on the "New" link on your Dashboard. Click on the "Internet of Things" links, then click on the "Azure IoT Hub" link.
![](../..//images/azure/create-new-iot-hub.png)

- Enter the information for your new Azure IoT Hub, then click on the "Create" button.
![](../..//images/azure/create-new-iot-hub-2.png)

- Your new Azure IoT Hub will be created in just a few moments.

## Obtain SAS token for administrative use

Once your Azure IoT Hub has been created, you need to obtain a SharedAccessSignature (SAS) token to perform adminitrativee actions such as creating or listing devices.

- Go to your Dashboard, then click on the link to your new Azure IoT Hub resource.
- Click on "Settings", then click on "Shared Access Policies".
![](../..//images/azure/obtain-sas.png)
- Obtain the ***host*** name and the ***primary key*** for the ***registryReadWrite*** policy.
- Create an SharedAccessSignature (SAS) token to be used to create and list devices. You can use the `sastoken` command line program like this:

```
sastoken <Your IoT Hub Name>.azure-devices.net/devices/ <Your primary key> 1440 registryReadWrite
```
For this to work you must be in the directory where you downloaded/cloned this example to. 
Ex (C:\Users\me\Documents\GitHub\intel-iot-examples-mqtt\support\azure\build\windows)

The program will output your SAS token as a string like this:

![](../..//images/azure/sas-example.png)


```
SharedAccessSignature sr=<Your IoT Hub Name>.azure-devices.net%2Fdevices&sig=<Super secret code>&se=<expiration timestamp>&skn=registryReadWrite
```

This SAS token will last for 1440 minutes before you will need to obtain a new one. It should be used only for administrative functions such as creating new devices. Each device will also need its own SAS token, which we will create in a subsequent step.
-You can find the `sastoken` program under `support\azure`. Precompiled binaries for Windows, OSX, and Linux can be found in the `support\azure\build` folder.

## Create new device

You can use ***curl*** to create a new device for your Azure IoT Hub using the SAS token with ***registryReadWrite*** access as obtained above. For example, to create a new device named "edison1":

```
$ curl -i -X PUT -H "Content-Type: application/json" -H "Authorization: <Your SharedAccessSignature>" -d "{deviceId: \"edison1\"}" https://<Your IoT Hub Name>.azure-devices.net/devices/edison1?api-version=2016-02-03
```
If you are using Windows* you may need to install [Cygwin*](https://github.com/hybridgroup/intel-iot-examples-mqtt/blob/feature/image-link/installing-cygwin.md) in order to use curl. 

You should receive a response like this:

![](../..//images/azure/create-new-device-curl.png)

```
HTTP/1.1 200 OK
Content-Length: 466
Content-Type: application/json; charset=utf-8
ETag: "MA=="
Server: Microsoft-HTTPAPI/2.0
Date: Thu, 24 Mar 2016 20:30:33 GMT

{"deviceId":"edison1","generationId":"635944482332920305","etag":"MA==","connectionState":"Disconnected","status":"enabled","statusReason":null,"connectionStateUpdatedTime":"0001-01-01T00:00:00","statusUpdatedTime":"0001-01-01T00:00:00","lastActivityTime":"0001-01-01T00:00:00","cloudToDeviceMessageCount":0,"authentication":{"symmetricKey":{"primaryKey":"<device primary key here>","secondaryKey":"<device secondary key here>"}}}
```

## Get list of devices

You can use ***curl*** to get the list of current devices for your Azure IoT Hub using the SAS token with ***registryReadWrite*** access as obtained above.

```
curl -i -H "Accept: application/json" -H "Authorization: <Your SharedAccessSignature>" https://<Your IoT Hub Name>.azure-devices.net/devices?api-version=2016-02-03
```

You should receive a response like this:

![](../..//images/azure/list-devices-curl.png)

```
HTTP/1.1 200 OK
Content-Length: 473
Content-Type: application/json; charset=utf-8
Server: Microsoft-HTTPAPI/2.0
Date: Fri, 25 Mar 2016 20:11:58 GMT

[{"deviceId":"edison1","generationId":"635944482332920305","etag":"MA==","connectionState":"Connected","status":"enabled","statusReason":null,"connectionStateUpdatedTime":"2016-03-25T05:28:11.4588528","statusUpdatedTime":"0001-01-01T00:00:00","lastActivityTime":"0001-01-01T00:00:00","cloudToDeviceMessageCount":0,"authentication":{"symmetricKey":{"primaryKey":"<device primary key here>","secondaryKey":"<device secondary key here>"}}}]
```
You will need the device's ***primaryKey*** or ***secondaryKey*** to obtain an SAS token for the device itself to connect to the Azure IoT Hub on its own.

## Obtain SAS token for device use

You need to create an SAS token for the Intel Edison to be able to connect to the Azure IotHub to record data. The SAS token for this purpose will have less priviliages then the SAS token created for admin use. You can use the `sastoken` command line program like this:

```
sastoken <Your IoT Hub Name>.azure-devices.net/devices/<Your device name> <Your device primary key> 1440
```

The program will output the SAS token for your device as a string like this:

![](../..//images/azure/device-sas-example.png)

```
SharedAccessSignature sr=<Your IoT Hub Name>.azure-devices.net%2Fdevices%2F<Your device name>&sig=<Super secret code>&se=<expiration timestamp>
```

This SAS token will last for 1440 minutes before you will need to obtain a new one. It should be used only for the specific device for which it was created. In other words, each device that you wish to connect will need its own SAS token.

## Summary of information

If you have followed all the steps outlined above, you now should have all of the information you will need to provide to your program so it can connect to the MQTT server:

MQTT_SERVER use "<Your IoT Hub Name>.azure-devices.net".

MQTT_CLIENTID use "<Your device name>".

MQTT_TOPIC use "devices/<Your device name>/messages/events/"

MQTT_USERNAME use "<Your IoT Hub Name>/<Your device name>".

MQTT_PASSWORD use the string with your device's SAS token.

## Additional setup for C++

## Additional setup for JavaScript

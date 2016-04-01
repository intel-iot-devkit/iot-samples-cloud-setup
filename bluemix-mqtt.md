# Connecting to IBM Bluemix Internet of Things Platform using MQTT

## IBM Bluemix IoT Platform Initial Setup

- Create an account on IBM Bluemix, if you do not yet have one.
![](https://github.com/hybridgroup/intel-iot-examples-mqtt/blob/master/images/ibm-bluemix/ibm-create-account.png)

- Login to your account
![](https://github.com/hybridgroup/intel-iot-examples-mqtt/blob/master/images/ibm-bluemix/ibm-login.png)

- Choose "New Dashboard" if that option is presented

## Add Internet of Things Platform

- Go to the "Dashboard"
- Click on "Use Services or APIs" to add a new service
![](https://github.com/hybridgroup/intel-iot-examples-mqtt/blob/master/images/ibm-bluemix/ibm-use-service-api.png)

- Click on "Internet of Things Platform"
![](https://github.com/hybridgroup/intel-iot-examples-mqtt/blob/master/images/ibm-bluemix/ibm-internet-of-things-platform.png)

- Enter "Service name"
![](https://github.com/hybridgroup/intel-iot-examples-mqtt/blob/master/images/ibm-bluemix/ibm-service-name.png)

- Choose a "Pricing Plan"
![](https://github.com/hybridgroup/intel-iot-examples-mqtt/blob/master/images/ibm-bluemix/ibm-pricing-plan.png)

- Click on the "Create" button
![](https://github.com/hybridgroup/intel-iot-examples-mqtt/blob/master/images/ibm-bluemix/ibm-hit-create.png)

## Add a device type

- Under "Connect your devices" click on the "Launch Dashboard" button. This will open a new window with your "IBM Watson IoT Platform" Dashboard.
![](https://github.com/hybridgroup/intel-iot-examples-mqtt/blob/master/images/ibm-bluemix/ibm-connect-launch.png)

- Click on the "Add Device" button.
![](https://github.com/hybridgroup/intel-iot-examples-mqtt/blob/master/images/ibm-bluemix/ibm-add-device-click.png)

- Click on the "Create device type" button. This will display the "Create Device Type" page.
![](https://github.com/hybridgroup/intel-iot-examples-mqtt/blob/master/images/ibm-bluemix/ibm-create-device-type.png)

- Click on the "Create device type" button.
![](https://github.com/hybridgroup/intel-iot-examples-mqtt/blob/master/images/ibm-bluemix/ibm-create-device-type2.png)

- Enter the "Name" and "Description", then click on the "Next" button.
![](https://github.com/hybridgroup/intel-iot-examples-mqtt/blob/master/images/ibm-bluemix/ibm-name-description.png)

- Define any attributes for your template, then click on the "Next" button.
![](https://github.com/hybridgroup/intel-iot-examples-mqtt/blob/master/images/ibm-bluemix/ibm-define-temp.png)

- On the "Submit Information" page, click on the "Next" button.
![](https://github.com/hybridgroup/intel-iot-examples-mqtt/blob/master/images/ibm-bluemix/ibm-submit.png)

- On the "Metadata" page, click on the "Create" button.
![](https://github.com/hybridgroup/intel-iot-examples-mqtt/blob/master/images/ibm-bluemix/ibm-meta.png)

## Add a device

- Select the new device type from the dropdown that you created in the previous step, then click on "Next".
![](https://github.com/hybridgroup/intel-iot-examples-mqtt/blob/master/images/ibm-bluemix/ibm-add-device-2.png)

- On the "Add Device" page, enter the "Device ID", then click on the "Next" button.
![](https://github.com/hybridgroup/intel-iot-examples-mqtt/blob/master/images/ibm-bluemix/ibm-device-id.png)

- On the "Metadata" page, click on the "Create" button.
![](https://github.com/hybridgroup/intel-iot-examples-mqtt/blob/master/images/ibm-bluemix/ibm-meta2.png)

- On the "Security" page, auto-generate an authentication token by just clicking on the "Next" button.
![](https://github.com/hybridgroup/intel-iot-examples-mqtt/blob/master/images/ibm-bluemix/ibm-sec-autogen.png)

- On the "Summary" page, click on the "Add" button.
![](https://github.com/hybridgroup/intel-iot-examples-mqtt/blob/master/images/ibm-bluemix/ibm-final-add.png)

- Make a note of the "Authentication Token" displayed on the "Your Device Credentials" page. Please note that "Authentication tokens are non-recoverable. If you misplace this token, you will need to re-register the device to generate a new authentication token"
![](https://github.com/hybridgroup/intel-iot-examples-mqtt/blob/master/images/ibm-bluemix/ibm-dont-lose-info.png)

## Summary of information

If you have followed all the steps outlined above, you now should have all of the information you will need to provide to your program so it can connect to the MQTT server:

MQTT_SERVER use "<Your organization ID>.messaging.internetofthings.ibmcloud.com", along with either the "ssl://" protocol in C++ or "mqtts://" protocol from JavaScript.

MQTT_CLIENTID use "d:<Your organization ID>:<Your device type>:<Your device ID>".

MQTT_TOPIC use "iot-2/evt/status/fmt/json"

MQTT_USERNAME use "use-token-auth".

MQTT_PASSWORD use the string with your device's authorization token.

## Additional setup for C++

When running your C++ code on the Edison, you need to set the MQTT parameters in Eclipse. Go to "Run configurations", and change the "Commands to execute before application" to the following:

```
chmod 755 /tmp/<Your app name>; export MQTT_SERVER="ssl://<Your organization ID>.messaging.internetofthings.ibmcloud.com:8883"; export MQTT_CLIENTID="d:<Your organization ID>:<Your device type>:<Your device ID>"; export MQTT_USERNAME="use-token-auth"; export MQTT_PASSWORD="<Your authorization token>"; export MQTT_TOPIC="iot-2/evt/status/fmt/json"
```

Click on the "Apply" button to save these settings.

Click on the "Run" button to run the code on the Edison.

## Additional setup for JavaScript

When running your JavaScript code on the Edison, you need to set the MQTT client parameters in the Intel XDK. You use the **config.json** file, by adding the following entries:

```
{
 "MQTT_SERVER": "mqtts://<Your organization ID>.messaging.internetofthings.ibmcloud.com:8883",
 "MQTT_CLIENTID": "d:<Your organization ID>:<Your device type>:<Your device ID>",
 "MQTT_USERNAME": "use-token-auth",
 "MQTT_PASSWORD": "<Your authorization token>",
 "MQTT_TOPIC": "iot-2/evt/status/fmt/json"
}
```

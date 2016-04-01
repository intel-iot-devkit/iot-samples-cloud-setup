# Connecting to IBM Bluemix Internet of Things Platform using MQTT

## IBM Bluemix IoT Platform Initial Setup

- Create an account on IBM Bluemix, if you do not yet have one.
![](https://github.com/hybridgroup/intel-iot-examples-mqtt/blob/master/images/ibm-bluemix/ibm-create-account.png)

- Login to your account
![](https://github.com/hybridgroup/intel-iot-examples-mqtt/blob/master/images/ibm-bluemix/ibm-login.png)

- Choose "New Dashboard" if that option is presented

## Add Internet of Things Platform

- Go to the "Dashboard"
- Click on "+" to add a new service
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


## Additional setup for C++

When running the code on the Edison, you need to set the following "Commands to execute before application":

```
chmod 755 /tmp/<Your app name>; export MQTT_SERVER="ssl://???:8883"; export MQTT_CLIENTID="???"; export MQTT_USERNAME="???"; export MQTT_PASSWORD="???"; export MQTT_TOPIC="???"
```

Click on the "Apply" button to save these settings.

Click on the "Run" button to run the code on the Edison.

## Additional setup for JavaScript

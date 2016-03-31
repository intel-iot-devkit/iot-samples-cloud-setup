# Connecting to IBM Bluemix Internet of Things Platform using MQTT

## IBM Bluemix IoT Platform Initial Setup

- Create an account on IBM Bluemix, if you do not yet have one.
- Login to your account
- Choose "New Dashboard" if that option is presented

## Add Internet of Things Platform

- Go to the "Dashboard"
- Click on "+" to add a new service
- Click on "Internet of Things Platform"
- Enter "Service name"
- Choose a "Pricing Plan"
- Click on the "Create" button

## Add a device type

- Under "Connect your devices" click on the "Launch Dashboard" button. This will open a new window with your "IBM Watson IoT Platform" Dashboard.
- Click on the "Add Device" button.
- Click on the "Create device type" button. This will display the "Create Device Type" page.
- Click on the "Create device type" button.
- Enter the "Name" and "Description", then click on the "Next" button.
- Define any attributes for your template, then click on the "Next" button.
- On the "Submit Information" page, click on the "Next" button.
- On the "Metadata" page, click on the "Create" button.

## Add a device

- Select the new device type from the dropdown that you created in the previous step, then click on "Next".
- On the "Add Device" page, enter the "Device ID", then click on the "Next" button.
- On the "Metadata" page, click on the "Create" button.
- On the "Security" page, auto-generate an authentication token by just clicking on the "Next" button.
- On the "Summary" page, click on the "Add" button.
- Make a note of the "Authentication Token" displayed on the "Your Device Credentials" page. Please note that "Authentication tokens are non-recoverable. If you misplace this token, you will need to re-register the device to generate a new authentication token"

## Additional setup for C++

When running the code on the Edison, you need to set the following "Commands to execute before application":

```
chmod 755 /tmp/<Your app name>; export MQTT_SERVER="ssl://???:8883"; export MQTT_CLIENTID="???"; export MQTT_USERNAME="???"; export MQTT_PASSWORD="???"; export MQTT_TOPIC="???"
```

Click on the "Apply" button to save these settings.

Click on the "Run" button to run the code on the Edison.

## Additional setup for JavaScript

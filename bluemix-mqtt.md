# Connecting to IBM\* Bluemix\* Internet of Things using MQTT\*

## IBM\* Bluemix\* IoT initial setup

1. Create an account on http://www.ibm.com/cloud-computing/bluemix, if you do not yet have one.
![](https://github.com/hybridgroup/intel-iot-examples-mqtt/blob/master/images/ibm-bluemix/ibm-create-account.png)

2. Log into your account.
![](https://github.com/hybridgroup/intel-iot-examples-mqtt/blob/master/images/ibm-bluemix/ibm-login.png)

3. Select **New Dashboard** if that option is presented.

## Add an Internet of Things Platform

1. Go to your dashboard and click **Use Services or APIs** to add a new service.
![](https://github.com/hybridgroup/intel-iot-examples-mqtt/blob/master/images/ibm-bluemix/ibm-use-service-api.png)

2. Click **Internet of Things Platform**.
![](https://github.com/hybridgroup/intel-iot-examples-mqtt/blob/master/images/ibm-bluemix/ibm-internet-of-things-platform.png)

3. In the **Service name** field, type a name of your choosing.
![](https://github.com/hybridgroup/intel-iot-examples-mqtt/blob/master/images/ibm-bluemix/ibm-service-name.png)

4. From the **Selected Plan** drop-down list, choose a pricing plan.
![](https://github.com/hybridgroup/intel-iot-examples-mqtt/blob/master/images/ibm-bluemix/ibm-pricing-plan.png)

5. Click the **Create** button.
![](https://github.com/hybridgroup/intel-iot-examples-mqtt/blob/master/images/ibm-bluemix/ibm-hit-create.png)

## Add a device type

1. Under **Connect your devices**, click the **Launch dashboard** button. <br> This opens a new **IBM Watson IoT Platform** window.
![](https://github.com/hybridgroup/intel-iot-examples-mqtt/blob/master/images/ibm-bluemix/ibm-connect-launch.png)

2. On the **Device Types** tile, click the **Add Device** button.
![](https://github.com/hybridgroup/intel-iot-examples-mqtt/blob/master/images/ibm-bluemix/ibm-add-device-click.png)

3. Click the **Create device type** button.<br> This opens the **Create Device Type** page.
![](https://github.com/hybridgroup/intel-iot-examples-mqtt/blob/master/images/ibm-bluemix/ibm-create-device-type.png)

4. Click the **Create device type** button.
![](https://github.com/hybridgroup/intel-iot-examples-mqtt/blob/master/images/ibm-bluemix/ibm-create-device-type2.png)

5. Fill the **Name** and **Description** fields and click **Next**.
![](https://github.com/hybridgroup/intel-iot-examples-mqtt/blob/master/images/ibm-bluemix/ibm-name-description.png)

6. Specify the attributes for your template and click **Next**.
![](https://github.com/hybridgroup/intel-iot-examples-mqtt/blob/master/images/ibm-bluemix/ibm-define-temp.png)

7. In the **Submit Information** step, click **Next**.
![](https://github.com/hybridgroup/intel-iot-examples-mqtt/blob/master/images/ibm-bluemix/ibm-submit.png)

8. If necessary, add metadata and click **Create**.
![](https://github.com/hybridgroup/intel-iot-examples-mqtt/blob/master/images/ibm-bluemix/ibm-meta.png)

## Add a device

1. From the **Choose Device Type** drop-down list, select the new device type you created in the previous section, and click **Next**.
![](https://github.com/hybridgroup/intel-iot-examples-mqtt/blob/master/images/ibm-bluemix/ibm-add-device-2.png)

2. In the **Device ID** field, type the ID of your device and click **Next**.
![](https://github.com/hybridgroup/intel-iot-examples-mqtt/blob/master/images/ibm-bluemix/ibm-device-id.png)

3. If necessary, add metadata and click **Next**.
![](https://github.com/hybridgroup/intel-iot-examples-mqtt/blob/master/images/ibm-bluemix/ibm-meta2.png)

4. In the **Security** step, auto-generate an authentication token by clicking **Next**.
![](https://github.com/hybridgroup/intel-iot-examples-mqtt/blob/master/images/ibm-bluemix/ibm-sec-autogen.png)

5. Click **Add**.
![](https://github.com/hybridgroup/intel-iot-examples-mqtt/blob/master/images/ibm-bluemix/ibm-final-add.png)

6. Make a note of the **Authentication Token** displayed under **Your Device Credentials**.
![](https://github.com/hybridgroup/intel-iot-examples-mqtt/blob/master/images/ibm-bluemix/ibm-dont-lose-info.png)

## Summary

If you have followed all the steps above, you should have all the information that your program needs to connect to the MQTT\* server:

`MQTT_SERVER` - use `\<Your organization ID\>.messaging.internetofthings.ibmcloud.com`, along with the `ssl://` (for C++) or `mqtts://` (for JavaScript*) protocol

`MQTT_CLIENTID` - use `d:\<Your organization ID\>:\<Your device type\>:\<Your device ID\>`

`MQTT_TOPIC` - use `iot-2/evt/status/fmt/json`

`MQTT_USERNAME` - use `use-token-auth`

`MQTT_PASSWORD` - use the string with the authorization token of your device

## Additional setup for C++

When running your C++ code on the Intel® Edison board, you need to set the MQTT\* client parameters in Eclipse\*. To do that:

1. Go to **Run configurations** and, in the **Commands to execute before application** field, type the following:

        chmod 755 /tmp/<Your app name>; export MQTT_SERVER="ssl://<Your organization ID>.messaging.internetofthings.ibmcloud.com:8883"; export MQTT_CLIENTID="d:<Your organization ID>:<Your device type>:<Your device ID>"; export MQTT_USERNAME="use-token-auth"; export MQTT_PASSWORD="<Your authorization token>"; export MQTT_TOPIC="iot-2/evt/status/fmt/json"

2. Click the **Apply** button to save these settings.
3. Click the **Run** button to run the code on your board.

## Additional setup for JavaScript*

When running your JavaScript\* code on the Intel® Edison board, you need to set the MQTT\* client parameters in the Intel® XDK IDE. Add the following entries to the **config.json** file:

        {
        "MQTT_SERVER": "mqtts://<Your organization ID>.messaging.internetofthings.ibmcloud.com:8883",
        "MQTT_CLIENTID": "d:<Your organization ID>:<Your device type>:<Your device ID>",
        "MQTT_USERNAME": "use-token-auth",
        "MQTT_PASSWORD": "<Your authorization token>",
        "MQTT_TOPIC": "iot-2/evt/status/fmt/json"
        }

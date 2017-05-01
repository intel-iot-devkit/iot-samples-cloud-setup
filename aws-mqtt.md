# Connecting to Amazon\* Web Services\* (AWS\*) IoT using MQTT*

## AWS* IoT initial setup

1. Create an account on https://aws.amazon.com, if you do not yet have one.
![](https://github.com/hybridgroup/intel-iot-examples-mqtt/blob/master/images/aws/aws-create-account.png)

2. Install the AWS\* CLI by following the instructions at http://docs.aws.amazon.com/cli/latest/userguide/installing.html.

### Adding the AWS\* CLI path to environment variables on Windows\*

1. Go to **Control Panel** and click **System**.
![](https://github.com/hybridgroup/intel-iot-examples-mqtt/blob/master/images/aws/aws-win-path-setup2.png)

2. Click **Advanced system settings**.
![](https://github.com/hybridgroup/intel-iot-examples-mqtt/blob/master/images/aws/aws-win-path-setup3.png)

3. On the **Advanced** tab, click **Environment Variables**.<br>
![](https://github.com/hybridgroup/intel-iot-examples-mqtt/blob/master/images/aws/aws-win-path-setup4.png)

4. In the **User variables for me** box, double-click **PATH**.
![](https://github.com/hybridgroup/intel-iot-examples-mqtt/blob/master/images/aws/aws-win-path-setup5.png)

5. Click **New**, add the full path to the AWS\* CLI installation directory, and click **OK**.
![](https://github.com/hybridgroup/intel-iot-examples-mqtt/blob/master/images/aws/aws-win-path-setup6.png)

6. In the **System variables** box, double-click **Path**.
![](https://github.com/hybridgroup/intel-iot-examples-mqtt/blob/master/images/aws/aws-win-path-setup7.png)

7. If the AWS\* CLI installation directory is not listed, repeat the actions from step 5.
![](https://github.com/hybridgroup/intel-iot-examples-mqtt/blob/master/images/aws/aws-win-path-setup8.png)

8. In the **Environment Variables** window, click **OK**.
![](https://github.com/hybridgroup/intel-iot-examples-mqtt/blob/master/images/aws/aws-win-path-setup9.png)

9. In the **System Properties** window, click **OK**.<br>
![](https://github.com/hybridgroup/intel-iot-examples-mqtt/blob/master/images/aws/aws-win-path-setup10.png)

    **Note:** For ease of use on Windows\*, while using the AWS\* CLI, follow the subsequent steps of this tutorial in the directory where you cloned this repository (for example, `C:\Users\me\Documents\GitHub\intel-iot-examples-mqtt\support\aws`).

Verify the setup by running this command:

    aws iot help

You should see the output like this:

![](https://github.com/hybridgroup/intel-iot-examples-mqtt/blob/master/images/aws/aws-verify-install.png)

## AWS permissions

If you run into any errors such as "AccessDeniedException" when trying to use the AWS CLI, they are likely due to insufficient permissions having been granted to the user account you are trying to use.

Make sure that the account has the needed IAM policy for access. Check out http://docs.aws.amazon.com/iot/latest/developerguide/iam-policies.html and make sure you have assigned them using http://docs.aws.amazon.com/IAM/latest/UserGuide/access_policies_managed-using.html

## Create a new device

To create a new device, use the `create-thing` command as follows:

    aws iot create-thing --thing-name "edison1"

You should see the output like this:

![](https://github.com/hybridgroup/intel-iot-examples-mqtt/blob/master/images/aws/aws-create-device.png)

## Get the list of devices

To list your devices, use the `list-things` command as follows:

    aws iot list-things

You should see the output like this:

![](https://github.com/hybridgroup/intel-iot-examples-mqtt/blob/master/images/aws/aws-list-things.png)

## Obtain and configure a certificate for device use

1. Provision a certificate:

        aws iot create-keys-and-certificate --set-as-active --certificate-pem-outfile cert.pem --public-key-outfile publicKey.pem --private-key-outfile privateKey.pem

    You should see the output like this:

    ![](https://github.com/hybridgroup/intel-iot-examples-mqtt/blob/master/images/aws/aws-provision-a-cert.png)

2. Create/attach policy:

        aws iot create-policy --policy-name "PubSubToAnyTopic" --policy-document file:///intel/how-to-code-samples/docs/mqtt/aws-device-policy.json

    You should see the output like this:

    ![](https://github.com/hybridgroup/intel-iot-examples-mqtt/blob/master/images/aws/aws-create-attach-policy.png)

3. Attach the certificate to a device:

    You need `certificate-arn` from step 1:

        aws iot attach-principal-policy --principal "certificate-arn" --policy-name "PubSubToAnyTopic"

## Determine the AWS* endpoint

You can obtain the **host** to use by running the following command:

    aws iot describe-endpoint

You should see the output like this:

![](https://github.com/hybridgroup/intel-iot-examples-mqtt/blob/master/images/aws/aws-determine-endpoint.png)

## Installing certificates to the Intel® Edison board

From your computer, run the following commands:

    scp -r cert.pem USERNAME@xxx.xxx.x.xxx:/home/root/.ssh
    scp -r publicKey.pem USERNAME@xxx.xxx.x.xxx:/home/root/.ssh
    scp -r privateKey.pem USERNAME@xxx.xxx.x.xxx:/home/root/.ssh

where `USERNAME@xxx.xxx.x.xxx` is the username and IP address you set for your board.

### Installing certificates to the Intel® Edison board (Windows* only)

We'll be using WinSCP* for the next steps. For installation instructions, refer to https://github.com/intel-iot-devkit/how-to-code-samples/blob/master/docs/cpp/using-winscp.md.

1. Log into your device using WinSCP*.
![](https://github.com/hybridgroup/intel-iot-examples-mqtt/blob/master/images/aws/aws-winscp1.png)

2. Make sure your host machine is in the directory where you ran your previous AWS\* CLI commands.
![](https://github.com/hybridgroup/intel-iot-examples-mqtt/blob/master/images/aws/aws-winscp2.png)

3. Copy **cert.pem**, **privateKey.pem**, and **publicKey.pem** to your `/home/root` directory on your Intel® Edison board.
![](https://github.com/hybridgroup/intel-iot-examples-mqtt/blob/master/images/aws/aws-winscp3.png)

## Summary

If you have followed all the steps above, you should have all the information that your program needs to connect to the MQTT* server:

`hostname` -  use the **host** value you obtained by running the `aws iot describe-endpoint` command, along with the `ssl://` (for C++) or `mqtts://` protocol (for JavaScript*)

`client_id` - use `[Your device name]`

`topic` - use `devices/[Your device name]`

`cert` - use the filename of the device certificate as described above

`key` - use the filename of the device key as described above

`ca` - use the filename of the CA certificate (`/etc/ssl/certs/VeriSign_Class_3_Public_Primary_Certification_Authority_-_G5.pem`)

## Additional setup for C++

When running your C++ code on the Intel® Edison board or Intel® IoT Gateway, you need to set the MQTT\* client parameters in Intel® System Studio\*. To do that:

1. Go to **Run configurations** and, in the **Commands to execute before application** field, type the following:

        export MQTT_SERVER="ssl://[hostname]:8883"; export MQTT_CLIENT_ID="[Your device ID]"; export MQTT_CERT="/home/root/.ssh/cert.pem"; export MQTT_CERT_KEY="/home/root/.ssh/privateKey.pem"; export MQTT_CA_ROOT="/etc/ssl/certs/VeriSign_Class_3_Public_Primary_Certification_Authority_-_G5.pem"; export MQTT_TOPIC="devices/[Your device ID]"

2. Click the **Apply** button to save these settings.
3. Click the **Run** button to run the code on your board.

## Additional setup for JavaScript*

When running your JavaScript\* code on the Intel® Edison board or Intel® IoT Gateway, you need to set the MQTT\* client parameters in the Intel® XDK IDE. Add the following entries to the **config.json** file:

```json
   "services": {
     "mqtt": {
       "hostname": "[your host name]",
       "client_id": "[device id]",
       "topic": "devices/[your device name]",
       "cert": "[device certificate filename]",
       "key": "[device key filename]"
    }
  }
```

## Additional setup for Python\*

When running your Python\* code on the Intel® Edison board or Intel® IoT Gateway, you need to use the MQTT interface by setting the client parameters. Add the following entries to the **config.json** file:

```json
   "services": {
     "mqtt": {
       "server": "[your host name]",
       "port": "8883",
       "client_id": "[device id]",
       "topic": "devices/[your device name]",
       "cert": "[device certificate filename]",
       "key": "[device key filename]"
    }
  }
```

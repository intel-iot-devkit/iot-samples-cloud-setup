# Connecting to Amazon Web Services IoT using MQTT

## AWS IoT Initial Setup

- Create an account on AWS, if you do not yet have one.
![](https://github.com/hybridgroup/intel-iot-examples-mqtt/blob/master/images/aws/aws-create-account.png)

- Install the AWS CLI, by following the instructions at http://docs.aws.amazon.com/cli/latest/userguide/installing.html

### For Windows* Users
- Be sure to add the aws path to your enviromental variables and always run the command prompt as administator.

### Adding the aws path to enviromental variables on Windows*

- Go to Start Menu

- Type control panel
![](https://github.com/hybridgroup/intel-iot-examples-mqtt/blob/master/images/aws/aws-win-path-setup1.png)

- Click on System
![](https://github.com/hybridgroup/intel-iot-examples-mqtt/blob/master/images/aws/aws-win-path-setup2.png)

- Click on Advanced system settings
![](https://github.com/hybridgroup/intel-iot-examples-mqtt/blob/master/images/aws/aws-win-path-setup3.png)

- Click on Enviromental Variables...
![](https://github.com/hybridgroup/intel-iot-examples-mqtt/blob/master/images/aws/aws-win-path-setup4.png)

- Under the "User variables for me box" click on PATH then click Edit...
![](https://github.com/hybridgroup/intel-iot-examples-mqtt/blob/master/images/aws/aws-win-path-setup5.png)

- Click "New" then add the directory you have installed awscli to. Then click "OK"
![](https://github.com/hybridgroup/intel-iot-examples-mqtt/blob/master/images/aws/aws-win-path-setup6.png)

- Under the "System variables box" selecte the "path" variable then click "Edit..."
![](https://github.com/hybridgroup/intel-iot-examples-mqtt/blob/master/images/aws/aws-win-path-setup7.png)

- If the enviromental path where you installed awscli is not there; Click "New" and add the directory and click "OK"
![](https://github.com/hybridgroup/intel-iot-examples-mqtt/blob/master/images/aws/aws-win-path-setup8.png)

- On the Eenviromental Variables" window click "OK"
![](https://github.com/hybridgroup/intel-iot-examples-mqtt/blob/master/images/aws/aws-win-path-setup9.png)

- On the System Properties window click "OK"
![](https://github.com/hybridgroup/intel-iot-examples-mqtt/blob/master/images/aws/aws-win-path-setup10.png)

- For ease of use on Windows* while using AWSCLI follow the next steps of this tutorial in the directory of which you cloned this repo
example (C:\Users\me\Documents\GitHub\intel-iot-examples-mqtt\support\aws)


- Verify setup, by running this command:
```
aws iot help
```
![](https://github.com/hybridgroup/intel-iot-examples-mqtt/blob/master/images/aws/aws-verify-install.png)

## Create new device

To create a new device, use the `create-thing` command like this:
```
aws iot create-thing --thing-name "edison1"
```

You should receive a response such as:
```
{
    "thingArn": "arn:aws:iot:us-west-2:974066999403:thing/edison1",
    "thingName": "edison1"
}
```
![](https://github.com/hybridgroup/intel-iot-examples-mqtt/blob/master/images/aws/aws-create-device.png)


## Get list of devices

To list your devices, use the `list-things` command like this:

```
aws iot list-things
```

You should receive a response such as:
```
{
    "things": [
        {
            "attributes": {},
            "thingName": "edison1"
        }
    ]
}
```
![](https://github.com/hybridgroup/intel-iot-examples-mqtt/blob/master/images/aws/aws-list-things.png)


## Obtain and configure certificate for device use

- Provision a certificate
```
aws iot create-keys-and-certificate --set-as-active --certificate-pem-outfile cert.pem --public-key-outfile publicKey.pem --private-key-outfile privateKey.pem
```

You should receive a response such as:
```
{
    "certificateArn": "arn:aws:iot:us-west-2:974066999403:cert/somelongidhere",
    "certificatePem": "-----BEGIN CERTIFICATE-----\n...\n-----END CERTIFICATE-----\n",
    "keyPair": {
        "PublicKey": "-----BEGIN PUBLIC KEY-----\n...\n-----END PUBLIC KEY-----\n",
        "PrivateKey": "-----BEGIN RSA PRIVATE KEY-----\n...\n-----END RSA PRIVATE KEY-----\n"
    },
    "certificateId": "somelongidhere"
}
```
![](https://github.com/hybridgroup/intel-iot-examples-mqtt/blob/master/images/aws/aws-provision-a-cert)



- Create/attach policy
```
aws iot create-policy --policy-name "PubSubToAnyTopic" --policy-document file:///intel/how-to-code-samples/docs/mqtt/aws-device-policy.json
```

You should receive a response such as:
```
{
    "policyName": "PubSubToAnyTopic",
    "policyArn": "arn:aws:iot:us-west-2:974066999403:policy/PubSubToAnyTopic",
    "policyDocument": "{\n    \"Version\": \"2012-10-17\",\n    \"Statement\": [{\n        \"Effect\": \"Allow\",\n        \"Action\":[\"iot:*\"],\n        \"Resource\": [\"*\"]\n    }]\n}\n",
    "policyVersionId": "1"
}
```
![](https://github.com/hybridgroup/intel-iot-examples-mqtt/blob/master/images/aws/aws-create-attach-policy.png)

- Attach certificate to device
You will need the "certificate-arn" from the "Provision a certificate" step:

```
aws iot attach-principal-policy --principal "certificate-arn" --policy-name "PubSubToAnyTopic"
```
![](https://github.com/hybridgroup/intel-iot-examples-mqtt/blob/master/images/aws/aws-attach-cert-to-device.png)


## Determine AWS endpoint

You can obtain the **host** to use by running the following command:

```
aws iot describe-endpoint
```
![](https://github.com/hybridgroup/intel-iot-examples-mqtt/blob/master/images/aws/aws-determine-endpoint.png)

## Installing certificates to the Edison

Run the following commands from your computer:

```
scp -r cert.pem USERNAME@xxx.xxx.x.xxx:/home/root/.ssh
scp -r publicKey.pem USERNAME@xxx.xxx.x.xxx:/home/root/.ssh
scp -r privateKey.pem USERNAME@xxx.xxx.x.xxx:/home/root/.ssh
```

Note that you must change `USERNAME@xxx.xxx.x.xxx` to match whatever username and IP address that you have set your board to.

### Installing certificates to the Edison (Windows* only)

- Use WINSCP for the next steps. [Installing WINSCP](https://github.com/intel-iot-devkit/how-to-code-samples/blob/master/docs/cpp/using-winscp.md)
- Log into your device using WINSCP
![](https://github.com/hybridgroup/intel-iot-examples-mqtt/blob/master/images/aws/aws-winscp1.png)

- Make sure your host machine is in the directory you ran your previous AWSCLI commands in
![](https://github.com/hybridgroup/intel-iot-examples-mqtt/blob/master/images/aws/aws-winscp2.png)

- copy "cert.pem", "privateKey.pem", and "publicKey.pem" to your /home/root directory on your Edison
![](https://github.com/hybridgroup/intel-iot-examples-mqtt/blob/master/images/aws/aws-winscp3.png)


## Summary of information

If you have followed all the steps outlined above, you now should have all of the information you will need to provide to your program so it can connect to the MQTT server:

MQTT_SERVER use the **host** value you determined by running the `aws iot describe-endpoint` command, along with either the "ssl://" protocol in C++ or "mqtts://" protocol from JavaScript.

MQTT_CLIENTID use "\<Your device name\>".

MQTT_TOPIC use "devices/\<Your device name\>"

MQTT_CERT use the filename of the device certificate as described above.

MQTT_KEY use the filename of the device key as described above.

MQTT_CA use the filename of the CA certificate `/etc/ssl/certs/VeriSign_Class_3_Public_Primary_Certification_Authority_-_G5.pem`

## Additional setup for C++

When running your C++ code on the Edison, you need to set the MQTT parameters in Eclipse. Go to "Run configurations", and change the "Commands to execute before application" to the following:

```
chmod 755 /tmp/<Your app name>; export MQTT_SERVER="ssl://<Your host name>:8883"; export MQTT_CLIENTID="<Your device ID>"; export MQTT_CERT="/home/root/.ssh/cert.pem"; export MQTT_KEY="/home/root/.ssh/privateKey.pem"; export MQTT_CA="/etc/ssl/certs/VeriSign_Class_3_Public_Primary_Certification_Authority_-_G5.pem"; export MQTT_TOPIC="devices/<Your device ID>"
```

Click on the "Apply" button to save these settings.

Click on the "Run" button to run the code on the Edison.

## Additional setup for JavaScript

When running your JavaScript code on the Edison, you need to set the MQTT client parameters in the Intel XDK. You use the **config.json** file, by adding the following entries:

```
{
 "MQTT_SERVER": "mqtts://<Your host name>:8883",
 "MQTT_CLIENTID": "<Your device ID>",
 "MQTT_CERT": "/home/root/.ssh/cert.pem",
 "MQTT_KEY": "/home/root/.ssh/privateKey.pem",
 "MQTT_TOPIC": "devices/<Your device ID>"
}
```

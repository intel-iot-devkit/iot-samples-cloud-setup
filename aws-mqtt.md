# Connecting to Amazon Web Services IoT using MQTT

## AWS IoT Initial Setup

- Create an account on AWS, if you do not yet have one.

- Install the AWS CLI, by following the instructions at http://docs.aws.amazon.com/cli/latest/userguide/installing.html

- Verify setup, by running this command:
```
aws iot help
```

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

- Attach certificate to device
You will need the "certificate-arn" from the "Provision a certificate" step:

```
aws iot attach-principal-policy --principal "certificate-arn" --policy-name "PubSubToAnyTopic"
```



## Determine AWS endpoint

You can obtain the **host** to use by running the following command:

```
aws iot describe-endpoint
```

## Installing certificates to the Edison

Run the following commands from your computer:

```
scp -r cert.pem USERNAME@xxx.xxx.x.xxx:/home/root/.ssh
scp -r publicKey.pem USERNAME@xxx.xxx.x.xxx:/home/root/.ssh
scp -r privateKey.pem USERNAME@xxx.xxx.x.xxx:/home/root/.ssh
```

Note that you must change `USERNAME@xxx.xxx.x.xxx` to match whatever username and IP address that you have set your board to.

## Summary of information

If you have followed all the steps outlined above, you now should have all of the information you will need to provide to your program so it can connect to the MQTT server:

MQTT_SERVER use the **host** value you determined by running the `aws iot describe-endpoint` command, along with either the "ssl://" protocol in C++ or "mqtts://" protocol from JavaScript.

MQTT_CLIENTID use "<Your device name>".

MQTT_TOPIC use "devices/<Your device name>"

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

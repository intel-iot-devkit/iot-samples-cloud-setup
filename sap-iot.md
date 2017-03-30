# Connecting to SAP\* Cloud Platform Internet of Things

To get started using the SAP\* IoT Starter Kit, you must first setup your account, by following the "Getting Started in the Cloud" steps listed at https://github.com/SAP/iot-starterkit#getting-started-in-the-cloud

## SAP\* Cloud Platform Developer signup

[Get SAP Cloud Platform Developer Account](https://github.com/SAP/iot-starterkit/src/prerequisites/account)
First, signup for an SAP Cloud Platform Developer account. If you already have an account, you can skip to the next step.

## SAP\* IoT setup

1. [Enable Internet of Things](https://github.com/SAP/iot-starterkit/src/prerequisites/service)
Next, enable the "Internet of Things" in your SAP Cloud Platform Cockpit.

2. [Create Device Information in Internet of Things Cockpit](https://github.com/SAP/iot-starterkit/src/prerequisites/cockpit)
To complete the needed prerequisites, you need to follow the following 3 steps:
   - Create a Message Type
   - Create a Device Type
   - Create a Device, and copy the generated Device Token. This is will be needed later to connect the Device.

3. [Deploy the Message Management Service (MMS)](https://github.com/SAP/iot-starterkit/src/prerequisites/mms)
Once the configuration is defined, the last step is to deploy the Message Management Service. Make sure that you perform role assignment of the `iotmms` application to your user account.

You should now have all of the information you need to connect your device to the SAP\* Cloud Platform Internet of Things.

## Summary

If you have followed all the steps above, you should have all the information that your program needs to connect to the SAP\* Cloud Platform Internet of Things:

info goes here...

## Additional setup for C++

When running your C++ code on the Intel® Edison board or Intel® IoT Gateway, you will need to use the RESTful client interface by setting the correct parameters in Eclipse\*. To do that:

1. Go to **Run configurations** and, in the **Commands to execute before application** field, type the following:

        info here...

2. Click the **Apply** button to save these settings.
3. Click the **Run** button to run the code on your board.

## Additional setup for JavaScript\*

When running your JavaScript\* code on the Intel® Edison board or Intel® IoT Gateway, you need to use the REST interface, by setting the client parameters in the Intel® XDK IDE. Add the following entries to the **config.json** file:

        goes here...

## Additional setup for Python\*

When running your Python\* code on the Intel® Edison board or Intel® IoT Gateway, you need to use the REST interface, by setting the client parameters. Add the following entries to the **config.json** file:

       goes here...

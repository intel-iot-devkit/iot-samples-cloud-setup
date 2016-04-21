#Installing Cygwin\* on Windows*

1. Go to https://cygwin.com.
![](https://raw.githubusercontent.com/intel-iot-devkit/how-to-code-samples/master/images/cpp/shell-script.png)

2. In the pane on the left, click **Install Cygwin**.
![](https://raw.githubusercontent.com/intel-iot-devkit/how-to-code-samples/master/images/cpp/shell-script2.png)

3. Download and run the executable file.<br> 
![](https://raw.githubusercontent.com/intel-iot-devkit/how-to-code-samples/master/images/cpp/shell-script3.png)

  **Important:** Use the default installation settings. Otherwise, if you install all the packages, it takes up to several gigabytes of free space.

4. Go to Windows\* **Control Panel** and click **System**.
![](https://raw.githubusercontent.com/intel-iot-devkit/how-to-code-samples/master/images/cpp/shell-script5.png)

5. Click **Advanced system settings**.
![](https://raw.githubusercontent.com/intel-iot-devkit/how-to-code-samples/master/images/cpp/shell-script6.png)

6. On the **Advanced** tab, click **Environment Variables**.<br>
![](https://raw.githubusercontent.com/intel-iot-devkit/how-to-code-samples/master/images/cpp/shell-script7.png)

7. In the **System variables** box, double-click **Path**.<br>
![](https://raw.githubusercontent.com/intel-iot-devkit/how-to-code-samples/master/images/cpp/shell-script8.png)

8. In the **Variable value** field, add the full path to the Cygwin\* installation directory (for example, `C\cygwin64\bin`) at the end of the line.<br>
![](https://raw.githubusercontent.com/intel-iot-devkit/how-to-code-samples/master/images/cpp/shell-script9.png)

  **Note:** Do not forget to add a semicolon (;) before the value to separate it from the other values.

Now you can use Linux\*/Unix\* commands in Windows\* Command Prompt.

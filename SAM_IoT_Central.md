# Connecting the SAM-IoT WG Development Board (Part No. EV75S95A) to Azure IoT Central

## Introduction

[Azure IoT Central](https://docs.microsoft.com/en-us/azure/iot-central/core/overview-iot-central) is an IoT application platform that reduces the burden and cost of developing, managing, and maintaining enterprise-grade IoT solutions. Choosing to build with IoT Central gives you the opportunity to focus time, money, and energy on transforming your business with IoT data, rather than just maintaining and updating a complex and continually evolving IoT infrastructure.

The web UI lets you quickly connect devices, monitor device conditions, create rules, and manage millions of devices and their data throughout their life cycle. Furthermore, it enables you to act on device insights by extending IoT intelligence into line-of-business applications.

## Program the Plug and Play Demo

1. Clone/download the MPLAB X demo project by issuing the following commands in a `Command Prompt` or `PowerShell` window

    ```bash
    git clone https://github.com/Azure-Samples/Microchip-SAM-IoT-Wx.git
    cd Microchip-SAM-IoT-Wx
    git submodule update --init
    ```

2. Launch the MPLAB X IDE and navigate to the main toolbar's `File` > `Open Project` operation to load the demo project folder (\*.X) located at `[your_path]\Microchip-SAM-IoT-Wx\firmware\AzureIotPnpDps.X`

   If the `load error` message in red appears in the `Output` window, click on the `Resolve DFP for configuration: default` link

    <img src=".//media/image21.png" style="width:6.5in;height:1.00833in" alt="A screenshot of a cell phone Description automatically generated" />

3. Set the `AzureIotPnpDps` project as the main (currently focused/active) project by right-clicking on it and selecting `Set as Main Project`

    <img src=".//media/image41.png" style="width:5.in;height:3.18982in" alt="A screenshot of a cell phone Description automatically generated" />

4. Verify the project properties are set correctly before building the
   project

    - Connect the board to PC, then make sure `CURIOSITY` device shows up as a disk drive in a `File Explorer` window

    - Right-click the project `AzureIotPnPDps` &gt; select `Properties` &gt; select "SAMD21-IoT WG-SN" for `Connected Hardware Tool` &gt; select the latest `SAMD21_DFP` version &gt; select the latest XC32 version for `Compiler Toolchain`. If any changes were made in the project properties window, the `Apply` button should become enabled.  Make sure to hit the `Apply` button before hitting `OK`

5. Build the project and set up a Command Line Interface (CLI) to the board

    - Open a serial terminal (e.g. PuTTY, TeraTerm, etc.) and connect to the COM port corresponding to your board at 9600 baud (e.g. open PuTTY Configuration window &gt; choose `session` &gt; choose `Serial`&gt; Enter the right COMx port). You can find the COM info by opening your PC’s `Device Manager` &gt; expand `Ports(COM & LPT)` &gt; take note of `Curiosity Virtual COM Port` 

        <img src=".//media/image27.png" style="width:3in;height:3in" alt="A screenshot of a cell phone Description automatically generated" />

    - Right-click the `AzureIotPnPDps` project and select `Make and Program Device`.  This operation will automatically clean and build the project before attempting to program the target device. After the `BUILD SUCCESSFUL` message appears in the Output window, the application HEX file will be programmed onto the SAM-IoT board. Once programming has finished, the board will automatically reset and start running its application code.

6. Before typing anything in the terminal emulator window, **disable** the local echo feature in the terminal settings for best results.  In the terminal window, hit `[RETURN]` to bring up the Command Line Interface prompt (which is simply the `>` character). Type `help` and then hit `[RETURN]` to get the list of available commands for the CLI.  The Command Line Interface allows you to send simple ASCII-string commands to set or get the user-configurable operating parameters of the application while it is running

    <img src=".//media/image44.png" style="width:5.in;height:3.18982in" alt="A screenshot of a cell phone Description automatically generated" />

7. In the terminal emulator window, set the debug messaging level to 0 to temporarily disable the output messages. Use the `debug <level>` command by manually typing it into the CLI.  The complete command must be followed by hitting `[RETURN]`
    ```bash
    >debug 0
    ```

8. Configure the SAM-IoT board's internal Wi-Fi settings with your wireless router’s SSID and password using the `wifi` command by manually typing it into the CLI.  The complete command must be followed by hitting `[RETURN]` (there cannot be any spaces used in the SSID or password)
    ```bash
    >wifi <NETWORK_SSID>,<PASSWORD>,<SECURITY_OPTION[1=Open|2=WPA|3=WEP]>
    ```
    For example, if the SSID of the router is "MyWirelessRouter" and the WPA/WPA2 key is "MyRoutersPassword", the exact command to type into the CLI (followed by `[RETURN]`) would be
    ```bash
    >wifi MyWirelessRouter,MyRoutersPassword,2
    ```
9. At the CLI prompt, type in the command `reset` and hit `[RETURN]` to restart the host application.  The Blue LED should eventually stay solidly ON to signify that the SAM-IoT board has successfully connected to the wireless router.
    ```bash
    >reset
    ```
## Create an IoT Central Application for your Device

IoT Central allows you to create an application dashboard to monitor the telemetry and take appropriate actions based on customized rules.

1. Create a custom IoT Central application by starting with an existing [Microchip IoT Development Board Template](https://apps.azureiotcentral.com/build/new/97343355-9171-4059-a68e-ae2383db6d01) (if there is a problem with loading the template, refer to the [Create an application](https://docs.microsoft.com/en-us/azure/iot-central/core/quick-deploy-iot-central) section to create your IoT Central application from scratch)

2. Review and select the settings for your IoT Central application (if needed, refer to [Create an application](https://docs.microsoft.com/en-us/azure/iot-central/core/quick-deploy-iot-central) for additional guidance on selecting the settings for your application). Click the `Create` button only after taking into consideration the following recommendations:
  
    - Choose a unique `Application name` which will result in a unique `URL` for accessing your application. Azure IoT Builder will populate a suggested unique `Application name` which can/should be leveraged, resulting in a unique `URL`.

        <img src=".//media/image80a.png" style="width:5.5.in;height:2.53506in" alt="A screenshot of a cell phone Description automatically generated" />

    - If you select the **Free** plan, you can connect up to 5 devices.  However, the free trial period will expire after 7 days which means a [paid pricing plan](https://azure.microsoft.com/en-us/pricing/details/iot-central/) will need to be selected to continue using the application.  Of course, there is nothing to stop you from creating a new free trial application but the device will need to be configured for the app from scratch.  Since the **Standard** plans each allow 2 free devices with no time-restricted trial period, if you only plan on evaluating 1 or 2 devices for connecting to the IoT Central app, then it's best to choose the **Standard 2** plan to get the highest total allowable number of messages (30K per month).

        <img src=".//media/image80b.png" style="width:6.5.in;height:3.63506in" alt="A screenshot of a cell phone Description automatically generated" />

    - `Billing info` section: If there is an issue with selecting an existing subscription in the drop-down list (or no subscriptions appear in the list at all), click on the `Create subscription` link to create a new subscription to use for the creation of this application.
    
        <img src=".//media/image80c.png" style="width:6.5.in;height:2.53506in" alt="A screenshot of a cell phone Description automatically generated" />

3. Create an X.509 enrollment group for your IoT Central application. If not already opened, launch your IoT Central application and navigate to `Administration` in the left pane and select `Device connection`

4. Select `+ Create enrollment group`, and create a new enrollment group using any name (Group type = `IoT devices`, attestation type = `Certificates (X.509)`).  Hit `Save` when finished

    <img src=".//media/image81.png" style="width:6.5.in;height:3.63506in" alt="A screenshot of a cell phone Description automatically generated" />

5. Now that the new enrollment group has been created, select `+ Manage Primary`.

    <img src=".//media/image82.png" style="width:5.5.in;height:2.53506in" alt="A screenshot of a cell phone Description automatically generated" />

6. Select the file/folder icon associated with the `Primary` field and upload the root certificate file `root-ca.crt` (located in the `ChainOfTrust` sub-folder that was created by the `SAM-IoT Provisioning Tools Package for Windows`).  The message "`(!) Needs verification`" should appear.  The `Subject` and `Thumbprint` fields will automatically populate themselves

    <img src=".//media/image75.png" style="width:5.5.in;height:3.13506in" alt="A screenshot of a cell phone Description automatically generated" />

7.	Click `Generate verification code` (this code will be copied to the clipboard which will be needed in a future step)

    <img src=".//media/image76.png" style="width:6.5.in;height:2.03506in" alt="A screenshot of a cell phone Description automatically generated" />

8. Open a Git Bash window (Start menu &gt; type `Git Bash`)

    <img src=".//media/image15.png" style="width:3.21739in;height:0.94745in" alt="A picture containing ball, clock Description automatically generated" />

9. Using the Git Bash command line, navigate to your certificates folder (the `ChainOfTrust` sub-folder which was generated by the [SAM-IoT Provisioning Tools Package for Windows](https://github.com/randywu763/sam-iot-provision))

    ```bash
    cd <path>\sam-iot-provision-main\SAM_IoT_Certs_Generator\ChainOfTrust
    ```

10. Execute the below command in the Git Bash window (copy and paste for best results)

    **Note**: Once you enter the below command, you will then be asked to enter information for various fields that will be incorporated into your certificate request. Enter the verification code (which was just generated previously) when prompted for the `Common Name`. It's recommended to just copy the `Verification code` to the clipboard and paste it when it's time to enter the `Common Name`.  For the rest of the fields, you can enter anything you want (or just hit `[RETURN]` to keep them blank which is fine for basic demonstration purposes).  If you accidentally hit `[RETURN]` when asked for the `Common Name`, you will need to run the command again...

    ```bash
    openssl req -new -key root-ca.key -out azure_root_ca_verification.csr
    ```
    <img src=".//media/image16.png" style="width:5.in;height:3.18982in" alt="A screenshot of a cell phone Description automatically generated" />

11. Generate the verification certificate by executing the following command exactly as shown (suggest copy and paste for best results)

    ```bash
    openssl x509 -req -in azure_root_ca_verification.csr -CA root-ca.crt -CAkey root-ca.key -CAcreateserial -out azure_signer_verification.cer -days 365 -sha256
    ```

12. Click `Verify` and select the `azure_signer_verification.cer` file to upload.  Confirm that the `Primary` certificate has been verified and that a `Thumbprint` has been generated for your certificate.  Click on `Close` to exit the current window, then click on `Save` at the top of the web application window.  The X.509 enrollment group has been successfully created and should be ready to go!

    <img src=".//media/image83.png" style="width:5.in;height:2.18982in" alt="A screenshot of a cell phone Description automatically generated" />

13. If not already active, launch a terminal emulator window and connect to the COM port corresponding to the SAM-IoT board at `9600` baud (**disable** local echo for the terminal settings for best results).  Hit `[RETURN]` to bring up the Command Line Interface prompt (which is simply the `>` character). Type `help` and then hit `[RETURN]` to get the list of available commands for the CLI.  The Command Line Interface allows you to send simple ASCII-string commands to set or get the user-configurable operating parameters of the application while it is running

    <img src=".//media/image44.png" style="width:5.in;height:3.18982in" alt="A screenshot of a cell phone Description automatically generated" />

14.	Look up the `ID Scope` for the DPS created/used by your IoT Central application (navigate to your application's web page and using the left-hand navigation pane, select `Administration` > `Device connection`).  The `ID Scope` will be programmed/saved into the [ATECC608A](https://www.microchip.com/wwwproducts/en/atecc608a) secure element on the board in the next step

    <img src=".//media/image84.png" style="width:5.in;height:3.18982in" alt="A screenshot of a cell phone Description automatically generated" />

15. In the terminal emulator window, hit `[RETURN]` to bring up the Command Line Interface prompt (which is simply the `>` character>). At the CLI prompt, type in the `idscope <your_ID_scope>` command to set it (which gets saved in the [ATECC608A](https://www.microchip.com/wwwproducts/en/atecc608a) secure element on the board) and then hit `[RETURN]`.  The ID Scope can be read out from the board by issuing the `idscope` command without specifying any parameter on the command line

    <img src=".//media/image85.png" style="width:5.in;height:3.18982in" alt="A screenshot of a cell phone Description automatically generated" />

16. In the terminal emulator window, hit `[RETURN]` to bring up the CLI prompt. Type in the command `reset` and hit `[RETURN]`

17. Wait for the SAM-IoT board to connect to your IoT Central application’s DPS; the Blue and Green LEDs will be flashing and/or staying on at different times/rates (which could take up to a few minutes).  Eventually the Blue and Green LEDs should both remain constantly ON.

    NOTE: If the Red LED comes on, then something may have been incorrectly programmed (e.g. wrong firmware, ID scope was entered incorrectly, etc.)

18. Go back to your web browser to access the Azure IoT Central application.  Use the left-hand side pane and select `Devices` > `All Devices`.  Confirm that your device is listed – the device name & ID is the Common Name of the device certificate (which should be `sn + {17-digit device ID}`)

    <img src=".//media/image86.png" style="width:5.in;height:1.38982in" alt="A screenshot of a cell phone Description automatically generated" />

19. If desired, change the Device name by clicking on `Manage device` > `Rename`

    <img src=".//media/image87.png" style="width:5.in;height:1.18982in" alt="A screenshot of a cell phone Description automatically generated" />

20. Click on the `Command` tab; type `PT5S` in the `Reboot delay` field and then click on `Run` to send the command to the device to reboot in 5 seconds

    <img src=".//media/image88.png" style="width:5.in;height:1.58982in" alt="A screenshot of a cell phone Description automatically generated" />

21. Within 5 seconds of sending the Reboot command, the SAM-IoT development board should reset itself.  Once the Blue and Green LED's stay constantly ON, press the SW0 and SW1 buttons (the Red LED may toggle with each button press)

    <img src=".//media/image89.png" style="width:5.in;height:1.28982in" alt="A screenshot of a cell phone Description automatically generated" />

22. Click on the `Raw data` tab and confirm that the button press telemetry messages were received

    <img src=".//media/image90.png" style="width:5.in;height:1.98982in" alt="A screenshot of a cell phone Description automatically generated" />

23. Click on the `Refresh` icon to display all messages received since the previous page refresh operation.  Confirm that periodic telemetry messages are being continuously received approximately every 10 seconds (the default interval value for the `telemetryInterval` property)

    <img src=".//media/image91.png" style="width:5.in;height:1.58982in" alt="A screenshot of a cell phone Description automatically generated" />

    <img src=".//media/image92.png" style="width:5.in;height:2.12982in" alt="A screenshot of a cell phone Description automatically generated" />

24. Increase the ambient light source shining on top of the board. Wait approximately 30 seconds.  Click on the `Refresh` icon to confirm that the light sensor value has increased

    <img src=".//media/image93.png" style="width:5.in;height:2.18982in" alt="A screenshot of a cell phone Description automatically generated" />

## Connect your Device to the Dashboard for Data Visualization

1. Navigate to the left-hand vertical toolbar and click on the `Dashboards` icon

    <img src=".//media/image100.png" style="width:5.in;height:0.98982in" alt="A screenshot of a cell phone Description automatically generated" />

2. Towards the top of the web page, click on the `Edit` icon

    <img src=".//media/image101.png" style="width:5.in;height:0.38982in" alt="A screenshot of a cell phone Description automatically generated" />

3. For **all** of the existing tiles named `Light` or `Temperature`, click on the upper right-hand corner of the tile to select `Configure`

    <img src=".//media/image102a.png" style="width:5.in;height:2.18982in" alt="A screenshot of a cell phone Description automatically generated" />
    <img src=".//media/image102b.png" style="width:5.in;height:2.18982in" alt="A screenshot of a cell phone Description automatically generated" />

4. Select `Device Group` > `SAM-IoT WM - All devices` and then check the box for your specific device name for `Devices`

    <img src=".//media/image103.png" style="width:5.in;height:2.08982in" alt="A screenshot of a cell phone Description automatically generated" />

5. Under the `Telemetry` category, click on `+ Capability` and select the parameter pertaining to the title of the tile (e.g. `Brightness from light sensor` for each of the `Light` tiles or `Temperature` for each of the `Temperature` tiles)

    <img src=".//media/image104a.png" style="width:5.in;height:0.89082in" alt="A screenshot of a cell phone Description automatically generated" />
    <img src=".//media/image104b.png" style="width:5.in;height:2.18982in" alt="A screenshot of a cell phone Description automatically generated" />
    <img src=".//media/image104c.png" style="width:5.in;height:1.18982in" alt="A screenshot of a cell phone Description automatically generated" />

6. Click on `Update` and repeat the process for the remainder of the existing tiles

    <img src=".//media/image105.png" style="width:5.in;height:0.48982in" alt="A screenshot of a cell phone Description automatically generated" />

7. After every tile has been configured to visualize your device's sensor data, click on the `Save` icon to save the latest changes to the dashboard

    <img src=".//media/image106.png" style="width:5.in;height:0.38982in" alt="A screenshot of a cell phone Description automatically generated" />

8. Confirm that the dashboard is being continuously updated with the expected telemetry data received from the device.  For example, adjust the ambient light source directed at the board and observe that the light sensor values are changing accordingly

    <img src=".//media/image107.png" style="width:5.in;height:2.58982in" alt="A screenshot of a cell phone Description automatically generated" />

8. To access your IoT Central application(s) in the future, go to [Azure IoT Central](https://apps.azureiotcentral.com) and click on `My apps`

    <img src=".//media/image108.png" style="width:5.in;height:1.98982in" alt="A screenshot of a cell phone Description automatically generated" />

## Expand the Dashboard with Additional Tiles

To create additional tiles for your IoT Central dashboard, refer to [Configure the IoT Central application dashboard](https://docs.microsoft.com/en-us/azure/iot-central/core/howto-add-tiles-to-your-dashboard). The below screen captures show additional possibilities of dashboard components that can highlight the telemetry data and properties facilitated by the `Plug and Play` interface.  Note that multiple devices can be selected for each tile to allow groups of devices to be visualized within a single tile. 

<img src=".//media/image95.png" style="width:5.in;height:3.34982in" alt="A screenshot of a cell phone Description automatically generated" />

<img src=".//media/image96.png" style="width:5.in;height:4.4182in" alt="A screenshot of a cell phone Description automatically generated" />

<img src=".//media/image97.png" style="width:5.in;height:3.34982in" alt="A screenshot of a cell phone Description automatically generated" />

<img src=".//media/image98.png" style="width:5.in;height:3.34982in" alt="A screenshot of a cell phone Description automatically generated" />

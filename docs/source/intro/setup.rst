Forward Loop Zero Setup Guide
=============================
This guide covers how to configure your Forward Loop Zero board for the first time.


Configure Wifi
--------------
If you plan to use wifi with Forward Loop Zero, you can configure your wifi credentials before you boot your hardware the first time.

For all Forward Loop Zero boards, you only need to edit one file in order to configure your wifi credentials. The specific file and its contents depend on which board you are using as part of Forward Loop Zero. 

For all boards, you should only need to perform the following steps one time. Then you can simply burn copies of the wifi-configured operating system onto new SD cards and boot multiple Forward Loop Zero boards that automatically connect to your wifi network.

:subscript:`To order Forward Loop Zero with your wifi credentials enabled out of the box, please contact us for information on custom deployment options at info@forward-loop.com`

.. tabs::

    .. group-tab:: Orange Pi Zero (Armbian)

        Remove the SD card from your Forward Loop Zero. Insert the SD card into your local machine or use an SD card reader to read the SD card on your local machine.

        In order to identify your wifi connection uniquely, you will need to generate a universally unique identifier (UUID) for the connection. You can do this on a Linux machine using the command `uuidgen -r` or you can generate one online from websites such as `https://www.uuidgenerator.net/ <https://www.uuidgenerator.net/>`_. Note that the UUID only needs to be unique for connections on a single Forward Loop Zero device. Different Forward Loop Zero devices can use the same UUID for identifying connections. 

        On the SD card, edit the file **/etc/NetworkManager/system-connections/YOUR_WIFI_ACCESS_POINT_NAME** where *YOUR_WIFI_ACCESS_POINT_NAME* is the name of your wifi network. Note that you will need *sudo* or privileged access in order to edit this file on the SD card.

        Add the following lines to the file, replacing *YOUR_WIFI_ACCESS_POINT_NAME* with your wifi network name, *YOUR_WIFI_PASSWORD* with your wifi password, and *RANDOM_UUID* with the UUID you generated above:

        .. code-block:: bash

            [connection]
            id=YOUR_WIFI_ACCESS_POINT_NAME
            uuid=RANDOM_UUID
            type=wifi
            permissions=
            secondaries=

            [wifi]
            mac-address-blacklist=
            mac-address-randomization=0
            mode=infrastructure
            seen-bssids=
            ssid=YOUR_WIFI_ACCESS_POINT_NAME

            [wifi-security]
            auth-alg=open
            group=
            key-mgmt=wpa-psk
            pairwise=
            proto=
            psk=YOUR_WIFI_PASSWORD

            [ipv4]
            dns-search=
            method=auto

            [ipv6]
            addr-gen-mode=stable-privacy
            dns-search=
            method=auto


    .. group-tab:: Raspberry Pi Zero W (Raspbian)

        Remove the SD card from your Forward Loop Zero. Insert the SD card into your local machine or use an SD card reader to read the SD card on your local machine.

        On the SD card, edit the file **/etc/wpa_supplicant/wpa_supplicant.conf**. Note that you will need *sudo* or privileged access in order to edit this file on the SD card.

        Edit the the file, replacing *YOUR_WIFI_ACCESS_POINT_NAME* with your wifi network name, *YOUR_WIFI_PASSWORD* with your wifi password, and *YOUR_COUNTRY_CODE* with `the wifi code for your country <https://www.arubanetworks.com/techdocs/InstantWenger_Mobile/Advanced/Content/Instant%20User%20Guide%20-%20volumes/Country_Codes_List.htm>`_:

        .. code-block:: bash

            ctrl_interface=DIR=/var/run/wpa_supplicant GROUP=netdev
            update_config=1
            country=YOUR_COUNTRY_CODE

            network={
                 ssid="YOUR_WIFI_ACCESS_POINT_NAME"
                 psk="YOUR_WIFI_PASSWORD"
                 key_mgmt=WPA-PSK
            }


Note that this guide assumes you use WPA encryption on your wifi network. We highly recommend you use WPA instead of WEP because WEP is known to be susceptible to password cracking attacks.

Now when you boot your Forward Loop Zero, it will connect to your wifi network.

Configure SSH
-------------
Once your Forward Loop Zero is connected to a network, you can log into the
device from your local machine over SSH. The default user is **floop** and the
default password is **floopfloop**. 

In order to improve the security of your device, you should configure your
device only to allow SSH authenticated with a key. In order to do this, we
recommend you follow the `floop Target Operating System Guide <https://docs.forward-loop.com/floopcli/master/intro/os.html#configuring-a-target-operating-system-to-meet-minimum-requirements>`_

Enable GPIO, UART, SPI, I2C
---------------------------
**When you purchase Forward Loop Zero with optional sensors and/or networking hardware, your Forward Loop Zero arrives already configured to work with your chosen hardware.**

If you want to use your Forward Loop Zero board with different sensors or hardware than those you ordered with your board, you may need to change some operating system settings in order to enable the other hardware. As a rule, Forward Loop Zero boards only come enabled with the minimum hardware necessary for full functionality with the sensors and hardware you ordered.

For all boards, you should only need to perform the following steps one time. Then you can simply burn copies of the hardware-configured operating system onto new SD cards and boot multiple Forward Loop Zero boards that connect to your specified hardware.

.. tabs::

    .. group-tab:: Orange Pi Zero (Armbian)

        Remove the SD card from your Forward Loop Zero. Insert the SD card into your local machine or use an SD card reader to read the SD card on your local machine.

        On the SD card, edit the file **/boot/armbianEnv.txt**.  Note that you may need *sudo* or privileged access in order to edit this file on the SD card.

        In this file, you should see a line that starts with:

        .. code-block:: bash

            overlays=

        You can enable or disable different hardware on the board by adding or removing the names of `device tree overlays <https://docs.armbian.com/User-Guide_Allwinner_overlays/>`_. For example, to enable the 3.3V and 5V I2C pins (i2c0 and i2c1) on the board, the line would be:

        .. code-block:: bash

            overlays=i2c0 i2c1

        The names for common hardware interfaces  and their `pin numbers <https://linux-sunxi.org/Xunlong_Orange_Pi_Zero#Expansion_Port>`_ are as follows:

        .. list-table::
            :widths: 25 25
            :header-rows: 1

            * - Name
              - Interface
            * - uart0 (always enabled)
              - Three-pin serial UART next to Ethernet port
            * - uart1
              - Serial UART (PG6, PG7, PG8, PG9)
            * - uart2
              - Serial UART (PA0, PA1, PA2, PA3)
            * - usbhost0
              - On-board USB port
            * - i2c0 
              - 3.3V I2C (PA11, PA12)
            * - i2c1
              - 5V I2C (PA18, PA19)
            * - spi-spidev
              - SPI device node (PA15, PA16, PA14, PA13) 
            

    .. group-tab:: Raspberry Pi Zero W (Raspbian)

        Remove the SD card from your Forward Loop Zero. Insert the SD card into your local machine or use an SD card reader to read the SD card on your local machine.

        On the SD card, edit the file **/boot/config.txt**.  Note that you may need *sudo* or privileged access in order to edit this file on the SD card.

        Make sure that you read the comments and documentation in this file to ensure that any changes you make result in valid hardware configurations.

        You can enable or disable different hardware on the board by adding or removing the names of `device tree overlays <https://www.raspberrypi.org/documentation/configuration/device-tree.md#part4.6>`_. 

        In order to enable or disable common hardware interfaces, simply add or remove `#` comments from lines that start with:

        .. code-block:: bash

            dtparam= 

        For example, to enable I2C on the board, make sure the file contains a line (with no `#` comment) that reads:

        .. code-block:: bash

            dtparam=i2c_arm=on

# Kerlink LoRa IoT Station with The Things Network

## Important
There are 2 version of stations:
1. LoRa IoT Station 
2. LoRa IoT Station + SPN

You need to know what version you are using in order to configure it correctly.
According to some posts on TTN forum and on Kerlink wiki, the 2nd version **(LoRa IoT Station + SPN)** doesn't work properly with firmware version 3.x because those firmware versions use SPN version 4.0. You'd not upgrade to those firmware versions or contact your support to tackle the issue.

In addition, you need to prepare:
- Two RJ45 ethernet cables (not provided in the box)
- WIRGRID debug tool (access to Linux console - **optional**)
- USB wire (A type <> B type) to connect debug probe to PC (**optional**)
- PoE injector (class 1, 48V) or DC power supply (10V to 30V) (provided in the box)
- An empty USB with FAT-32 format.

## How to setup gateway hardware
1. After unpacking the Kerlink IoT Station, open the case by putting a screwdriver in the top notch (where the antenna is located). 

    ![Open the station](/img/1.jpg)

2. Connect a RJ45 network cable on the green connector, cable colors are noted next to the connector. You can use an existing cable by cutting of the connector of one side, or you need to make a new cable including attaching the connector (watch the coloring scheme).

    ![RJ45 connection](/img/4.jpg)

3. Plug the cable in **DATA & POWER OUT** port of the power adapter. Connect the **DATA IN** port of the power adapter to your existing network. If you use PoE switches, the power adapter is not needed.

    ![Test and Leds](/img/2.jpg)

4. Ensure you connect gateway antena to the gateway before powering it up because it can cause some damages. After powering on, the LEDs inside the gateway do not work by default, they only work for about a minute after shortly pressing the “Test” button. This includes the power LEDs.

    ![RJ45 matching](/img/3.jpg)
    
5. If you’d like to use a GPRS/3G connection, insert the SIM-card now.
    * Remove the SIM card holder of the Lora IoT Station by pressing the extraction button with a little screwdriver.
    * Place the SIM card in the SIM card holder.
    * Carefully insert the SIM card holder with the SIM card in the LoRa IoT Station.
    * Configure the settings for the SIM card.
    
        ![SIM card](/img/5.png)
## How to connect to the gateway using SSH connection
### Purpose
This section is to know how for connecting to the gateway for further configuration such as: `Firmware upgrade` and `Package forwarder installation`.
### Instructions
1. Default IP address for the gateway is **192.168.4.155** so you have to configure the PC ethernet address in the same LAN (**192.168.4.150** for example) in order to be able to connect to it.
2. Start an ssh client (typically Putty on Windows) on port 22. After opening the prompt for Login/password below:
    * Until firmware version *wirmaV2_wirgrid_v2.x(.y)*  
        ```
        Welcome On Kerlink Wirgrid Board
        Wirgrid_0b03007f login:
        ```
        * after getting prompt: enter login “root” with password “root”.
        * firmware version wirmaV2_wirgrid_v2.x(.y) is currently used in production (version out of the box).
        * The keyword “Wirgrid” is useful to identify the firmware version in the prompt.
    * For firmware version *wirnet_v3.1*
         ```
        Welcome On Kerlink Wirnet Station
        Wirnet_0b03007f login
        ```
        * After getting prompt : enter login “root” with password “pdmk-0$serialno_last_7_characters_lowercase”.
        * The serial number is available in the console prompt, on the product sticker (door opened) and the delivery notice.
        * Firmware version wirnet_v3.1 is available for download on the Resources Wirnet Station Firmware.
        * The keyword “Wirnet” is useful to identify the firmware version in the prompt
            ```
            To prevent Web robots to attack the station with standard login/password as “root/root”, default password is now built using the 7 last characters of the product serial number in lower case, prefixed by “pdmk-0”, with the following template : pdmk-0$serialno_last_7_characters_lowercase.

            For example, if the wirnet station serial number is 0xB0E032D, then the root password will be pdmk-0b0e032d
            
            This 7 characters can be retrieved in the hostname, also displayed in the shell prompt:
            Welcome On Kerlink Wirnet Station
            Wirnet_0b0e032d login: root
            Password: pdmk-0b0e032d
            ```
    * Since firmware version *wirnet_v3.2*
        ```
        The hostname, since v3.2, is in capital letters, so the password changed in capital letters also.
        ```
        ```
        Welcome On Kerlink Wirnet Station
        Wirnet_0B03007F login
        ```
        * After getting prompt : enter login “root” with password “pdmk-0$serialno_last_7_characters_uppercase”.
        * The serial number is available in the console prompt, on the product sticker (door opened) and the delivery notice.
        * Firmware version wirnet_v3.2 is available for download on the Resources Wirnet Station Firmware.
        * The keyword “Wirnet” is usefull to identify the firmware version in the prompt.
        
            ```
            To prevent Web robots to attack the station with standard login/password as “root/root”, default password is now built using the 7 last characters of the product serial number in capital letters, prefixed by “pdmk-0”, with the following template : pdmk-0$serialno_last_7_characters_uppercase.

            For example, if the wirnet station serial number is 0xB0E032D, then the root password will be pdmk-0B0E032D
            
            This 7 characters can be retrieved in the hostname, also displayed in the shell prompt:
            Welcome On Kerlink Wirnet Station
            Wirnet_0B0E032D login: root
            Password: pdmk-0B0E032D
            ```
            
## How to upgrade gateway firmware (optional)
### Notice
* The firmware programming steps can be followed with the debug probe. If you are not familiar with this method, please log the console output in a file (with the teraterm terminal for example) to send to the support team in case of troubles and support request.
* Please find all neccessary files [here](http://wikikerlink.fr/lora-station/doku.php?id=wiki:resources).
### Requirements
* The archive of the firmware, usbflashdrive_wirmav2_wirxxx_v<version number>.zip, downloadable from Firmware resources page [here](http://wikikerlink.fr/lora-station/doku.php?id=wiki:resources).
* The archive of the **Produsb file**, **Produsb_wirxxx_v<version number>.zip**, downloadable from Firmware resources page.
* An empty USB Flash drive 1.1 compatible, formatted in FAT32.

### Restrictions
* Do not reset the Wirnet™ Station during the upgrade process.
* Do not power off the Wirnet™ Station during the upgrade process.
* Do not press the reset button during the upgrade process.
* Do not unplug the ethernet cable or remove SIM card during the upgrade process.
* Validation of this process must be done by the customer before triggering the migration campaign on site.

### Steps
* Unzip the archives into the root directory of an empty USB flash drive USB1.1 compatible, formatted in FAT32.
* The flash drive content will be as following (example with wirnet v3.3 firmware): 
    ```
    USB:/
    ├── initramfs.cpio.gz.uboot
    ├── produsb.sh
    ├── rootfs.tar.gz
    ├── script_uboot.img
    ├── u-boot.bin
    ├── uImage
    ├── userfs.tar
    ├── wirmaV2_wirnet_v3.1.md5
    └── wirmaV2_wirnet_v3.1.txt 
     
    0 directories, 9 files
    ```
    * **Step 1** - Power ON the Wirnet™ Station. Make sure the system is stable (no periodic reboot, linux shell available). Do not interrupt the power, and do not press the RESET button during the whole process.
    * **Step 2** - Press the TEST button to activate the leds (during 1 minute).
    * **Step 3** - Plug the USB flash drive into the Wirnet™ Station USB slot.
    * **Step 4** - Wait for 10 seconds, the USB stick will be detected and the upgrade will be triggered.
    * **Step 5** - The leds “MOD1” and “MOD2” will blink alternatively during the upgrade (around 3 minutes, you can push again the TEST button to check the leds activity, DO NOT press the RESET button!
    * **Step 6** - The gateway will reboot, “MOD1” and “MOD2” will be powered OFF.
    * **Step 7** - You can access to the shell prompt via the debug probe or SSH.
    * **Step 8** - Unplug the USB flash drive, the file produsb.log is generate/updated and is a lock file to avoid to trigger an other upgrade of this gateway.
        ```
        When the knetID (serial number) is in the file produsb.log, the upgrade request is ignored.
        ```
    * **Step 9** - The programming process is over.
    * **Step 10** - Check firmware version again.
        ```
        Get the firmware version of your LoRa IoT Station (Wirnet Station) with the command “get_version -u -v”
        ```
        
## How to configure TTN Package Forwarder
### How to install software
* You need to download either [The Things Network’s packet forwarder (EU version)](https://github.com/TheThingsNetwork/kerlink-station-firmware/blob/master/dota/dota_thethingsnetwork_v1.3_EU.tar.gz) from TTN Github and correct produsb.zip version (To match with gateway firmware) from [Kerlink wiki](http://wikikerlink.fr/lora-station/doku.php?id=wiki:resources).
* Follow this procedure to update or replace the packet forwarder:
    1. Copy `produsb.sh` (from extracting produsb.zip) and the `dota_thethingsnetwork_<version>_EU.tar.gz` onto an empty USB flash drive formatted in FAT-32. Make sure there is no .log file.
    2. Plug the USB flash drive into the gateway.
    3. Wait for 5 min. During this time the gateway will reboot itself.
    4. Unplug the key and check that a `.log` file has appeared. The file should contain `WirmaV2 0x080XXXXX updated`. This log file prevents any further installation on the gateways to avoid cyclic reboots.
    5. To redo the update on same gateway, remove this log file from the flash drive reinsert it into the gateway USB. This is not needed if you update another gateway.

### How to disable Firewall to enable downlink
* When using the **wirnet_v3.1+** firmware, the firewall is activated by default. This could disable the gateway from sending downlink. To solve this, we can simply disable the firewall:
    1. Open the file `/etc/sysconfig/network`.
    2. Change `FIREWALL=yes` to `FIREWALL=no`.
    3. reboot the gateway.

* Doublecheck: After the installation, there should be a folder `/mnt/fsuser-1/thethingsnetwork` containing:
    * `global_conf.json`.
    * `local_conf.json`: You can find **Gateway_ID** here which is the one you need to register on TTN network (tick option `I'm using the legacy packet forwarder` on TTN when you register this gateway).
    
        ![Gateway registeration](/img/6.png)

    * Check if you're gateway is on after the registration
    
        ![Gateway check](/img/7.png)
     
## How to configure Cellular network on the gateway
1. **SIM card detection is only done at boot time**. Insert the SIM card in the powered off LoRa station.
2. Set your APN settings in `/etc/sysconfig/network` with your network operational parameters:

    ``` 
    You can open the file via the terminal, type: vi /etc/sysconfig/network. 
    Type i to start editing the file, after doing so, save and quit the file: press esc + : + wq + enter
    ```
    
3. Adapt your network configuration params to the file:

    ```
    # Selector operator APN
    GPRSAPN=m2minternet
    # Enter pin code if activated
    GPRSPIN=
    # Update /etc/resolv.conf to get dns facilities
    GPSDNS=yes
    # PAP authentication
    GPRSUSER=kerlink
    GPRSPASSWORD=password
    
    # Bearers priority order
    BEARERS_PRIORITY="ppp0,eth0,eth1"
    ```
    
4. Configure the **autoconnect** in `/knet/knetd.xml`. All params should be the same with no more extra lines.

    ```
    <!-- ############## local device configuration ############## -->
    <LOCAL_DEV role="KNONE"/>
    
    <!-- ############## connection parameters ############## -->
    <!-- enable the autoconnect feature (YES/NO) -->
    <CONNECT auto_connection="YES" />
    <!-- frequency of connection monitoring -ping- (in seconds) -->
    <CONNECT link_timeout="30"/>
    <!-- DNS servers will be pinged if commented or deleted. Some operators can block the ping on there DNS servers -->
    <CONNECT ip_link="8.8.8.8"/>
    
    <!-- ############## default area for connection policy ############## -->
    
    <AREA id="default">
    <ACCESS_POINT bearer="gprs" />
    </AREA>  
    ```
    
5. There is a bug in the software. When `GPRSUSER` and `GPRSPASSWORD` stay empty, the gateway is not able to setup cellular connection. To resolve this problem, please modify file `/etc/init.d/gprs` at this line:

     ```
     [ ${GPRSUSER} ] && PPP_OPTIONS="user ${GPRSUSER} password ${GPRSPASSWORD}"
     ```
     
6. Another bug is the gateway goes offline and doesn’t restart automatically when the cellular connection drops. A workaround is to restart the packet forwarder when this occurs. You can do so by adding the line: `/usr/bin/killall` `poly_pkt_fwd` at the bottom of the files `/etc/ppp/ip-up` and `/etc/ppp/ip-down`.

7. Check status via LEDs on the station. GSM1 and GSM2 should be on or you can simply also observe package tranmissions with the command below:

    ```
    # tcpdump -i ppp0 -v
    ```
    
8. Please find [here](https://www.thethingsnetwork.org/docs/gateways/kerlink/cellular.html) for the full instruction from TTN.
    
## How to get product version in case you need to provide for support
1. Connect to the gateway station using SSH connection
2. Get the firmware version of your LoRa IoT Station (Wirnet Station) with the command “**get_version -u -v**” 
3. The firmware version is after the field **PROD_FW=**

    ```
    Welcome On Kerlink Wirgrid Board
    Wirgrid_0b03007f login: root
    Password: root
    [root@Wirgrid_0b03007f /root]# get_version -u -v
    KERNEL_VER=3.10.37-klk-25ef920
    PIC_VER=8.3
    BOOTSTRAP_VER=
    UBOOT_VER=1618
    SCRIPT_VER=1618
    INITRAMFS_VER=ga2c2b25
    FILESYSTEM_VER=2011.08-g1935e04
    KNETD_VER="wirma2_v4.06 WAN_3.16 (Aug 12 2016-11:19:00)"
    PROD_FW=wirmaV2_wirgrid_v2.3.1
    LORABOARD_MANUFACTURER=00
    LORABOARD_TYPE="868-27dBm"
    LORABOARD_HWVERSION=05
    LORABOARD_SERIALNO=001614
    
    ************************ Update log ************************
    1970.01.01-00:01:52 -- Production with wirmaV2_wirgrid_v2.3.1
    ************************************************************
    [root@Wirgrid_0b03007f /root]#
    ```
## References
1. [The Things Network](https://www.thethingsnetwork.org/docs/gateways/kerlink/)
2. [Kerlink Wiki](http://wikikerlink.fr/lora-station/doku.php?id=lorahome)

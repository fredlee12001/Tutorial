# README

The full documentation about mbed-cloud-client-example is [available on our documentation site](https://cloud.mbed.com/docs/current/connecting/device-management-client-tutorials.html)

## UNO_91H porting for PDMC.

This document contains the instructions for how to use this repository to make RDA5981 connect to Pelion Device Management platform.
It was originally based on the [mbed-cloud-client-example](https://github.com/ARMmbed/mbed-cloud-client-example) repo. While we modified little bit to work with RDA5981 Wifi SOC. 

* [Pre-Requirement](#pre)
* [Fetch and Config](#fetch)
* [Build](#build)
* [Work with Provisioning](#Prov)

<h2 id="pre">Pre-Requrirement </h2>

1. The mbed development environment - [Mbedcli](https://github.com/ARMmbed/mbed-cli)

2. Compile tools installed. For example: [GCC_ARM](https://launchpad.net/gcc-arm-embedded/)

3. Mbed cloud accounts. [account](https://www.pelion.com/docs/device-management/current/account-management/index.html)

4. HW ready.  [UNO_91H](https://os.mbed.com/platforms/UNO-91H/) Wifi SOC (Previously RDA5981x)

<h2 id="fetch">Fetch code and Config the build parameter</h2>
You could change the config by the [mbed_app.json](./mbed_app.json) file in the root folder.

1. Fetch the code. 

    
    ```sh
    mbed import dmc-china-movtek-smartlock
    cd dmc-china-movtek-smartlock
    ```


2. Config the connnectivities.

    In the mbed_app.json, there is option for WIFI default config. 
    
    ```sh
         "nsapi.default-wifi-ssid"                   : "\"SSID\"",
         "nsapi.default-wifi-password"               : "\"PASSWORD\""
    ```
    Modify according to your environment.


3. Config the developer Mode. 

    pelion device management client support two mode. One for developer usage, another is for production/deployment.
    By default, it's configured for developer. 
    In the mbed_app.json, Find below option. 
    
    ```sh
        "config": {
        "developer-mode": {
            "help"      : "Enable Developer mode to skip Factory enrollment",
            "options"   : [null, 1],
            "value"     : 1
        },
    ```
    Modify the value accordingly to switch the mode.

4. Preparing the code for PDMC. 
   ```sh
   cp .mbedignorePDMC .mbedignore
   ```

<h2 id="build">Build the binaries. </h2>


1. By default, we use developer mode, following [Link](https://www.pelion.com/docs/device-management/current/connecting/provisioning-development-devices.html#creating-and-downloading-a-developer-certificate) to get your credential file. 

    
2. Compile the target binary. It will generate MK-PDMC.bin finally.

    ```sh
    mbed compile -t GCC_ARM -m UNO_91H -n MK-PDMC
    ```

<h2 id="Prov">Provision support. </h2>

1.  Preparing the code for FCC.


    cp .mbedignoreFCC .mbedignore



2.  Compile the binary.  It will generate MK-FCC.bin


    mbed compile -t GCC_ARM -m UNO_91H --app-config=FCC_UNO91H_1M.json -DFCE_SERIAL_INTERFACE -N MK-FCC


3.  Following the standard provisioning process to do the factory provisioning. 

**Note::You must turn off developer mode inside PDMC to use with provisioning.**

## References.

1. Mbed Cloud [Documents](https://cloud.mbed.com/docs/v1.2).
2. Mbed OS [Documents](https://os.mbed.com/docs/mbed-os/v5.12/introduction/index.html).

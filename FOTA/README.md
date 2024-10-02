# FOTA Update with Adrastea-I

## Introduction
The Adrastea-I module supports FOTA over TCP and can be used to remotely update the firmware of the device. This document explains the steps and AT commands required to perform a FOTA update on the Adrastea-I module using an HTTP server.
### Key Features
- **Firmware Update Protocol**: HTTP
- **Transport**: TCP
- **FOTA Server**: User-configured server where the firmware update file (update.ua) is hosted.

The following commands guide you through the process of configuring the Adrastea-I module for the FOTA update. Each command is expected to return an “OK” response unless specified otherwise.

## Before FOTA Update:
The upgradeable firmware should be a delta change with the base of the current firmware. For this, load the BASE firmware onto the Adrastea (MAP-Base.bin)
The Base Firmware has the following details:

```bash
MCU_02_02_11_00_33421_LO
MAP Compiled on Jan 26 2024 at 15:16:41

MCU menu -- MAP
========
help or ? - Print this help menu
read addr - Read address (hex format)
write addr value - Write value (hex) to address (hex)
ps - Print task list
meminfo - Reports memory information
ver - Show complitaion date and time
hifc [status [clr]] [timer MS] [sus] [res]
serialinfo [serial num]
time [show | set Seconds(from epoch)]]
reset - Reset all cores
hibernate - <enable|disable|show>
map - pipeline to MAP via internal UART
```
Now, we need to update the firmware on top of this firmware through a FOTA procedure

### 1. Clearing Previous HTTP Configurations
Before configuring a new HTTP session, clear any existing configurations:

```bash
AT%HTTPCFG="CLEAR",3
```

### 2. Enabling HTTP Event Notification
The following command enables HTTP event notifications from the Adrastea-I module:

```bash
AT%HTTPEV="ALL",1
```

### 3. Configuring the HTTP Node
Configure the HTTP node with the URL of the HTTP server where the FOTA update file (update.ua) is stored. Replace xxxx with the actual server address:

```bash
AT%HTTPCFG="NODES",3,"http://xxxx/fota/update.ua"
```

### 4.  Re-enable HTTP Event Notifications
To ensure event notifications are active during the update, re-enable HTTP event notifications:

```bash
AT%HTTPEV="ALL",1
```

### 5. Setting the Request Format
Set the request format to ensure proper communication during the FOTA process:

```bash
AT%HTTPCFG="FORMAT",3,0,0,0
```

### 6. Initiating the FOTA Download
To initiate the FOTA download from the configured HTTP server, use the following command:

```bash
AT%HTTPFOTAGET=3
```

Expected Output:
```bash
OK
%HTTPEVU:"FOTADLRES",3,0
```
The FOTADLRES response indicates that the firmware download has been successfully initiated.

### 7. Reboot the system
After the firmware has been downloaded, reboot it to observe the changes
```bash
ATZ
```

## After FOTA Update:
The upgraded firmware should be a delta change with the base of the current firmware.
The Updated Firmware has the following details:

```bash
MCU_02_02_11_00_33421_LO
MAP Compiled on Jan 26 2024 at 15:21:20

MCU menu -- MAP
========
help or ? - Print this help menu
read addr - Read address (hex format)
write addr value - Write value (hex) to address (hex)
ps - Print task list
meminfo - Reports memory information
ver - Show complitaion date and time
hifc [status [clr]] [timer MS] [sus] [res]
serialinfo [serial num]
time [show | set Seconds(from epoch)]]
reset - Reset all cores
hibernate - <enable|disable|show>
map - pipeline to MAP via internal UART
test_message - prints out message
```
We can observe that the binary file from the updated firmware was compiled at a different time compared to the Base firmware and that we have a new command "test_message" at the end.
---
title: "My First Post"
date: 2024-09-27T09:56:49-05:00
draft: false
---

# Lecture: Syslog

## Overview

- Syslog is an industry standard protocol for message logging
- On network devices, Syslog can be used to log events such as changes in interface status (up/down), changes in OSPF neighbor status, system restarts, etc.
- The messages can be displayed in the CLI, saved in the devices RAM, or sent to an external Syslog server
- Logs are essential when troubleshooting issues, examining the cause of incident, etc.
- Syslog and SNMP are both used for monitoring and troubleshooting devices. They are complementary, but their functionalities are different  
    <br/>

## Message Format

`seq:time stamp: %facility-severity-MNEMONIC:description`

- **Sequence number** - indicates the order/sequence of messages
- **Timestamp** - indicates the time the message was generated
    - These two fields may or may not be displayed depending on the device's configuration
- **Facility** - indicates which process on the device generate the message
- **Severity** - indicates the severity level of the logged event (8 levels)
- **Mnemonic** - short code for the message indicating what happened
- **Description** - detailed information about the event being reported and what actually happened  
    <br/>

## Severity Levels

| Level | Keyword | Description |
| :--- | :--- | :--- |
| 0   | Emergency | System is unusable |
| 1   | Alert | Action must be taken immediately |
| 2   | Critical | Critical conditions |
| 3   | Error | Error conditions |
| 4   | Warning | Warning conditions |
| 5   | Notice/Notification | Normal but significant condition |
| 6   | Informational | Informational messages |
| 7   | Debugging | Debug-level messages |

- Severity are subjective, so it's by vendor interpretation
- Every Awesome Cisco Engineer Will Need Ice cream Daily  
    <br/>

## Message Examples

<img src=":/bd571b7379de4e0a9d1876f84c6a5515" alt="711e58b5a2005ba64d47280b8b2b18a5.png" width="918" height="359" class="jop-noMdConv">

## Logging Locations

- **Console line** - Syslog messages will be displayed in the CLI when connected to the device via the console port. By default, all messages (level 0 - 7) are displayed
- **VTY lines** - Sysolg messages will be displayed in the CLI when connected to the device via Telnet/SSH; disabled by default
- **Buffer** - Syslog messages will be saved to RAM. By default, all message levels are displayed
    - You can view message in buffer with `show logging`
- **External server** - You can configure the device to send Syslog messages to an external server
    - Syslog servers will listen for messages on UDP port 514  
        <br/>

## Syslog Configurations

*Note: This is all done in global config mode*

- `logging console <level-number/keyword>`
    - Configures logging to the console
    - You can use the number (6) or keyword (informational)
    - This will enable logging for the informational severity level and higher
- `logging monitor <level-number/keyword>`
    - Configures logging to VTY lines (remote shell)
- `logging buffered [size-bytes] <level-number/keyword>`
    - Configures logging to buffer
    - Size is optional, will use default, in this case specified 8192
- `logging <ip-addr>` or `logging host <ip-addr>`
    - Configures logging to external server
    - `logging trap <level-number/keyword>`
        - Next command to set the logging level for the external server

### Terminal Monitor

- Even if **logging monitor** level is enabled, by default Syslog messages will not be displayed when connected via Telnet or SSH
- `terminal monitor` in privileged exec mode
    - This command must be used every time you connect to the device via Telnet or SSH

### Logging Synchronous

- By default, logging messages are displayed in the CLI while you are in the middle of typing
- `logging synchronous`
    - Should be on set on the appropriate line to disable this
    - Will cause a new line to be printed with your previous keystrokes if your typing is interrupted by a message

### Service Timestamps & Service Sequence-Numbers

- `service timestamps log <datetime/uptime>`
    - Enables timestamps in Syslog messages
    - **datetime** displays the date/time when the event occurred
    - **uptime** displays how long the device had been running when the event occurred
- `service sequence-numbers`
    - Enables sequence numbers in Syslog messages  
        <br/>

## Command Review

`logging console <severity>`  
`logging monitor <severity>`  
`logging buffered [size] <severity>`  
`logging <server-ip>`  
`logging host <server-ip>`  
`logging trap <severity>`  
`terminal monitor`  
`logging synchronous`  
`service timestamps log [datetime | uptime]`  
`service sequence-numbers`  
<br/>

## Syslog vs SNMP

- Syslog and SNMP are both used for monitoring and troubleshooting of devices. They are complementary, but their functionalities are different
- In Syslog, messages are sent from the devices to the server. The server can't actively pull or change information on devices like SNMP can
- SNMP is used to retrieve and organize information about the SNMP managed devices  
    <br/>

# Quiz

1.  What is the severity level of the following Syslog message?  
    Feb 11 09:41:00.554: %SYS-5-CONFIG_I: Configured from console by jeremy on console  
    a) System  
    **c) Notification**  
    d) Warning  
    b) Informational  
    e) Alert  
    f) Debugging  
    g) Config  
    <br/>
2.  What is the severity level of the following Syslog message?  
    `*Feb 11 03:02:55.304: %LINK-3-UPDOWN: Interface GigabitEthernet0/0, changed state to up`  
    a) Emergency  
    b) Error  
    c) Notification  
    d) Notice  
    **e) Critical**  
    f) Notice  
    g) Warning  
    <br/>
3.  Which of the following locations are Syslog messages sent to by default, without any specific Syslog configuration? (select two)  
    a) External Syslog server  
    **b) Console line  
    c) Buffer**  
    d) VTY lines  
    <br/>
4.  You issue the `logging buffered 6` command on R1. Syslog messages of which severity levels will be saved to the logging buffer?  
    a) All Syslog messages  
    b) Severity 6 and 7  
    **c) Severity 0 to 6**  
    d) Severity 6 only  
    <br/>
5.  Which of the following Syslog message fields might not be displayed, depending on the device’s configuration? (select two)  
    `seq:time stamp: %facility-severity-MNEMONIC:description`  
    **a) seq**  
    b) facility  
    c) severity  
    **d) time stamp**  
    <br/>
6.  Match keywords to severity levels  
    0 = emergencies (Every)  
    1 = alerts (Awesome)  
    2 = critical (Cisco)  
    3 = errors (Engineer)  
    4 = warnings (Will)  
    5 = notifications (Need)  
    6 = informational (Ice cream)  
    7 = debugging (Daily)  
    <br/>

# Lab

![d7f1f1ae96eaac817168708d8b450f8a.png](:/34de71df9df34075b10f1c4953442225)  
*R1 username: jeremy, PW: ccna, enable PW: ccna*

### 1. Connect to R1's console port using PC2:
 -Shut down the G0/0 interface
 -After you receive a syslog message, re-enable the interface.
 -What is the severity level of the syslog messages?
 -Enable timestamps for logging messages


- PC2
    - Terminal > OK > login
    - `interface g0/0` and `shutdown`
    - `no shutdown`
    - Severity level 5: Notification
    - `service timestamps log datetime msec`  
        <br/>

### 2\. Telnet from PC1 to R1's G0/0 interface (watch the video to learn how!)
 -Enable the unused G0/1 interface
 -Why does no syslog message appear?
 -Enable logging to the VTY lines for the current session.
  *there is no 'logging monitor' command in packet tracer, but it's enabled by default
  
- PC1
    - Command prompt > `telnet 192.168.1.1`
    - `interface g0/1` and `no shutdown`
    - No Syslog message appeared because you need to enable it using a command for every session
    - `terminal monitor` in privileged exec mode  
        <br/>

### 3. Enable logging to the buffer, and configure the buffer size to 8192 bytes.

- PC1 (Telnet through command prompt)
    - `logging buffered 8192`  
        <br/>

### 4. Enable logging to the syslog server SRV1 with a level of 'debugging'.

- PC1 (Telnet through command prompt)
    - `logging 192.168.1.100`
    - `logging trap debugging`
- SRV1
	- Services > Syslog to view logs
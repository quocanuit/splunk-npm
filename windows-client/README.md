## Install and Configure Active Directory

- In Window Clients:
1. Search This PC > Properties > Advanced system settings
2. Click Computer name > Change
3. Click checkbox Domain in Member of section
4. Enter domain name to join domain

## Setup Sysmon and Splunk Universal Forwarder

- Install Sysmon version 15.15 [here](https://download.sysinternals.com/files/Sysmon.zip)
- Install Splunk Universal Forwarder [here](https://www.splunk.com/en_us/download/universal-forwarder.html)


#### Configure Universal Forwarder

Save configuration files in the same folder:

- Install sysmon using command:

```
.\Sysmon64.exe -i ..\sysmonconfig.xml
```
- Install Universal Forwarder with Receiving Indexer as Splunk server

- Navigate to the directory containing the configuration files for the Universal forwarder
```
cd C:\Program Files\SplunkUniversalForwarder\etc\system\local
```
- Add inputs.conf and outputs.conf file to send log to Splunk server
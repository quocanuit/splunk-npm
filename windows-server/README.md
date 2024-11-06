## Install and Configure Active Directory

- In Window Server:
1. Navigate to the Windows Server Manager
2. Click Manage > Add Roles and Features
3. Select the server from the server pool and click Next
4. Select Active Directory Domain Services
5. Click Add Features
6. Click Install on the Confirmation window
7. After install ADDS, promote this server to a Domain Controller

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
## Send logs from pfSense and Suricata to Splunk

### 1. pfSense setup

- Set up WAN and LAN interfaces to 172.20.10.8 and 192.168.72.5

- Access pfSense web configurator via LAN interface IP address, then Enable Secure Shell
- Enable Remote Logging, Remote log servers: 192.168.72.139:5147
- Install Suricata, configure Suricata to enable rules to download: ETOpen and Snort Community rule base
> Snort Rules Filename: community-rules.tar.gz
- Add rules to WAN interface, let Suricata log in EVE JSON mode, then select the rule sets.
- Install Splunk Forwarder on pfSense, config /opt/splunkforwarder/etc/system/local/[outputs.conf](./outputs.conf); /opt/splunkforwarder/etc/apps/TA-Suricata/default/[inputs.conf](./TA-Suricata/inputs.conf)
> Suricata interface from `/var/log/suricata/`
### 2. Splunk setup

1. Create 2 indexes: network, ids
2. Install [Splunk Common Information Model (CIM)](../splunk-common-information-model-cim_532.tgz), [TA-pfSense](../ta-pfsense_251.tgz), [Splunk TA for Suricata](../splunk-ta-for-suricata_233.tgz)
3. Create Data Inputs: UDP, port 5147, type "pfsense", host "pfSense.home.arpa", index "network"
4. `index=network sourcetype=pfsense*`, `index=ids sourcetype=suricata*`
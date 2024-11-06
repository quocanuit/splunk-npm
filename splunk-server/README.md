### Active Directory Dashboard queries

```
index=endpoint host="ADDC" collection=CPU counter="% Processor Time" 
| timechart span=10s avg(Value) as "CPU Usage %" 
| fillnull value=0 
| eval alert=if('CPU Usage %' > 20, "Warning: High CPU Usage", "Normal") 
| stats latest(alert) as current_alert by _time 
| sort -_time
```
```
index=endpoint host="ADDC" collection=CPU counter="% Processor Time" 
| timechart span=10s avg(Value) as "CPU Usage %" 
| fillnull value=0
```
```
index=endpoint host="ADDC" collection=Memory counter="Page Faults/sec" 
| timechart span=10s avg(Value) 
| fillnull value=0
```
```
index=endpoint host="ADDC" collection=LogicalDisk (counter="Disk Read Bytes/sec" OR counter="Disk Write Bytes/sec") 
| timechart span=10s avg(Value) by counter 
| fillnull value=0
```
```
index=endpoint host="ADDC" collection="Network Interface" instance="Intel[R] 82574L Gigabit Network Connection" (counter="Bytes Received/sec" OR counter="Bytes Sent/sec") 
| bucket _time span=10s 
| stats avg(Value) as Value by _time, counter 
| timechart span=10s useother=false avg(Value) by counter
```
```
index=endpoint host="ADDC" collection="Network Interface" instance="Intel[R] 82574L Gigabit Network Connection"
| bucket _time span=1m
| stats avg(Value) as avg_value by counter
| eval avg_value=round(avg_value,2)
| eval metric=counter." : ".avg_value
| table metric
```
```
index="endpoint" host="TARGET-PC" EventCode=5156 
| rex field=Message "Destination Port:\s+(?P<DestPort>[^\r\n]+)" 
| eval Protocol=case(
    DestPort="80", "HTTP",
    DestPort="443", "HTTPS",
    DestPort="53", "DNS",
    DestPort="389", "LDAP",
    DestPort="3389", "RDP",
    DestPort="445", "SMB",
    DestPort="88", "Kerberos",
    1=1, "Other"
)
| stats count by Protocol
```
```
index="endpoint" host="TARGET-PC" EventCode=5156
| rex field=Message "Source Address:\s+(?P<SourceAddress>[^\r\n]+)"
| top limit=10 SourceAddress
| eval percent=round(percent,2)
```
```
index=endpoint host="TARGET-PC"
| stats count as event_count
| eval status=if(event_count > 0, "Active", "Inactive")
| table status
```
### pfSense Dashboard queries
```
index=network sourcetype="pfsense:filterlog" 
| eval bytes=coalesce(bytes_in, bytes_out, 0)
| timechart span=10s sum(bytes) as "Traffic Volume"
| fillnull value=0
```
```
index=network sourcetype="pfsense:filterlog" 
| timechart span=10s count as "Connections" 
| fillnull value=0
```
```
index=ids sourcetype=suricata alert.severity=* 
| timechart span=10s count by alert.severity useother=false
| fillnull value=0
```
```
index=network sourcetype="pfsense:filterlog" 
| timechart span=10s count by action useother=false
| fillnull value=0
```
```
index=network sourcetype="pfsense:filterlog" 
| where match(src_ip, "^\d{1,3}\.\d{1,3}\.\d{1,3}\.\d{1,3}$")
| stats count by src_ip 
| sort -count 
| head 7
```
```
index=network sourcetype="pfsense:filterlog" 
| where match(dest_ip, "^\d{1,3}\.\d{1,3}\.\d{1,3}\.\d{1,3}$")
| stats count by dest_ip 
| sort -count 
| head 7
```
```
index=network sourcetype="pfsense:filterlog" 
| stats count by dest_port 
| sort -count 
| head 7
```
```
index="network" sourcetype="pfsense:filterlog" 
| eval bytes=coalesce(bytes_in, 0) + coalesce(bytes_out, 0) 
| timechart span=10s sum(bytes) as total_bytes 
| eval throughput_mbps=(total_bytes*8)/1000000 
| fields _time, throughput_mbps 
| rename throughput_mbps as "Throughput (Mbps)"
```
```
index="network" sourcetype="pfsense:filterlog"
| eval bytes=coalesce(bytes, 0)
| timechart span=5m sum(bytes) as total_bytes
| eval "Throughput (Bps)"=round(total_bytes/300, 2)
| stats max("Throughput (Bps)") as peak_throughput
| eval peak_throughput=round(peak_throughput, 2)
```
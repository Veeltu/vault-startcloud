

1. Citrix - https://edp.50hertz.com/logon/LogonPoint/tmindex.html

2. Open JH - RDP:
10.89.46.85
10.89.46.86
Login and Pass for VPN

3. Open grafana:
https://gw.observability.test.pndrs.de
Login and Password for grafana

caxim8

### browser MiB
https://mibbrowser.online/mibdb_search.php

https://github.com/prometheus/snmp_exporter/blob/16185db0f330e5e6eadf8cd83741cde80320edfe/snmp.yml#L42025
https://github.com/open-telemetry/opentelemetry-collector-contrib/tree/main/receiver/tcpcheckreceiver


te wartosci dodac do grafany :
```
auth:
              - vyos
            # we may still need to filter these
            module:
              # SNMPv2-MIB
              # https://mibbrowser.online/mibdb_search.php?mib=SNMPv2-MIB
              # https://github.com/prometheus/snmp_exporter/blob/16185db0f330e5e6eadf8cd83741cde80320edfe/snmp.yml#L42025
              - system

              # IF-MIB
              # https://mibbrowser.online/mibdb_search.php?mib=IF-MIB
              # https://github.com/prometheus/snmp_exporter/blob/16185db0f330e5e6eadf8cd83741cde80320edfe/snmp.yml#L22378
              - if_mib

              # HOST-RESOURCES-MIB
              # https://mibbrowser.online/mibdb_search.php?mib=HOST-RESOURCES-MIB
              # https://github.com/prometheus/snmp_exporter/blob/16185db0f330e5e6eadf8cd83741cde80320edfe/snmp.yml#L22342
              - hrSystem
              - hrSWRunPerf
              - hrStorage

              # UCD System Stats
              # https://mibbrowser.online/mibdb_search.php?mib=UCD-SNMP-MIB
              # https://github.com/prometheus/snmp_exporter/blob/16185db0f330e5e6eadf8cd83741cde80320edfe/snmp.yml#L44975C3-L44975C19
              - ucd_system_stats
              
              # Cisco FC
              # https://mibbrowser.online/mibdb_search.php?mib=UCD-SNMP-MIB
              # https://github.com/prometheus/snmp_exporter/blob/16185db0f330e5e6eadf8cd83741cde80320edfe/snmp.yml#L44975C3-L44975C19
              - cisco_fc_fe
```
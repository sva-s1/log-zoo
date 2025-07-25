# Fortinet FortiGate Logs

This directory contains sample log files from Fortinet FortiGate firewalls. These logs are transported via syslog but use Fortinet's proprietary key-value format rather than standard syslog message formats.

## Directory Structure

```
fortinet/
└── event.type/
    ├── security-rating/
    │   └── fortigate.log
    ├── system/
    │   └── fortigate.log
    └── traffic/
        └── fortigate.log
```

The logs are organized by event type to facilitate easy categorization and analysis. Additional event types can be added following the same structure pattern.

## Log Format

Fortinet logs use a space-separated key-value pair format where each field is represented as `key=value`. The logs contain no structured delimiters like JSON or XML, making them unique among network security appliances.

### Common Fields

All Fortinet logs share these common fields:

- `date` - Log date (YYYY-MM-DD format)
- `time` - Log time (HH:MM:SS format)  
- `devname` - Device name/hostname
- `devid` - Unique device identifier/serial number
- `eventtime` - High-precision timestamp in nanoseconds
- `tz` - Timezone offset
- `logid` - Unique log identifier for the event type
- `type` - Primary log category
- `subtype` - Secondary log classification
- `level` - Log severity level
- `vd` - Virtual domain name

## Event Types

### Traffic Logs (`event.type/traffic/`)

Network traffic logs that record connection attempts, sessions, and policy decisions.

**Key Fields:**
- `srcip`/`dstip` - Source and destination IP addresses
- `srcport`/`dstport` - Source and destination ports
- `srcintf`/`dstintf` - Source and destination interfaces
- `proto` - IP protocol number
- `action` - Policy action (allow, deny, etc.)
- `policyid` - Applied security policy ID
- `service` - Network service/application
- `sentbyte`/`rcvdbyte` - Bytes transmitted/received
- `duration` - Session duration

### System Logs (`event.type/system/`)

System-level events including DHCP statistics, interface status, and administrative actions.

**Key Fields:**
- `logdesc` - Human-readable log description
- `interface` - Network interface name
- `msg` - Additional message details
- `total`/`used` - Resource utilization metrics (for DHCP, memory, etc.)

### Security Rating Logs (`event.type/security-rating/`)

Security posture assessment and compliance reporting events.

**Key Fields:**
- `auditid` - Unique audit session identifier
- `audittime` - Audit execution timestamp
- `auditscore` - Overall security score
- `auditreporttype` - Type of security assessment
- `criticalcount`/`highcount`/`mediumcount`/`lowcount` - Vulnerability counts by severity
- `passedcount` - Number of passed security checks

## Transport Method

These logs are typically transported via:
- **Syslog** (UDP/514, TCP/514, or TLS/6514)
- **OFTP** (Fortinet's proprietary protocol)
- **Log forwarding** to FortiAnalyzer or FortiManager

## Parsing Considerations

When parsing Fortinet logs:

1. **Key-Value Parsing**: Split on spaces, then on `=` for each key-value pair
2. **Quoted Values**: Some values may be enclosed in double quotes
3. **IPv6 Support**: IP addresses can be IPv4 or IPv6 format
4. **Timestamp Handling**: Multiple timestamp formats (date/time, eventtime nanoseconds)
5. **Field Variability**: Not all fields appear in every log entry
6. **Escaping**: Special characters in values may be escaped

## Sample Log Entries

### Traffic Log
```
date=2025-07-25 time=07:43:43 devname="FortiGate-40F-SVA" devid="FGT40FTK2409BDPZ" eventtime=1753454623318460180 tz="-0700" logid="0001000014" type="traffic" subtype="local" level="notice" vd="root" srcip=fe80::2a70:4eff:fe71:34f1 srcport=5353 srcintf="wan" srcintfrole="wan" dstip=ff02::fb dstport=5353 dstintf="root" dstintfrole="undefined" sessionid=99717 proto=17 action="deny" policyid=0 policytype="local-in-policy6" service="udp/5353" trandisp="noop" app="udp/5353" duration=0 sentbyte=0 rcvdbyte=0 sentpkt=0 rcvdpkt=0 appcat="unscanned"
```

### System Log
```
date=2025-07-25 time=07:58:50 devname="FortiGate-40F-SVA" devid="FGT40FTK2409BDPZ" eventtime=1753455529570272359 tz="-0700" logid="0100026003" type="event" subtype="system" level="information" vd="root" logdesc="DHCP statistics" interface="lan" total=101 used=0 msg="DHCP statistics"
```

### Security Rating Log
```
date=2025-07-25 time=06:57:11 devname="FortiGate-40F-SVA" devid="FGT40FTK2409BDPZ" eventtime=1753451831097278960 tz="-0700" logid="0110052000" type="event" subtype="security-rating" level="notice" vd="root" logdesc="Security Rating summary" auditid=1753451798960 audittime=1753451831 auditscore="265.0" auditreporttype="PostureReport" criticalcount=2 highcount=2 mediumcount=3 lowcount=0 passedcount=17
```

## Additional Event Types

Fortinet devices can generate many other event types including:

- **UTM Logs**: Antivirus, web filtering, application control, IPS
- **VPN Logs**: SSL VPN and IPSec tunnel events  
- **Authentication Logs**: User login/logout events
- **Administrative Logs**: Configuration changes and admin actions
- **HA Logs**: High availability cluster events
- **Wireless Logs**: WiFi access point and client events

## References

- [Fortinet Log Message Reference](https://docs.fortinet.com/)
- [FortiGate Administration Guide](https://docs.fortinet.com/product/fortigate)
- [Log Format Documentation](https://docs.fortinet.com/document/fortigate/7.4.0/log-message-reference)

# RFC 5424 - The Syslog Protocol

This directory contains log samples that follow **RFC 5424**, the modern Syslog Protocol standard. RFC 5424 represents a significant evolution from RFC 3164, providing enhanced capabilities for structured logging and improved internationalization support.

## About RFC 5424

RFC 5424 was published in March 2009 as a replacement for RFC 3164. It addresses many limitations of the original BSD syslog format while maintaining backward compatibility where possible.

### Key Improvements over RFC 3164

- **Structured data elements** for standardized metadata
- **Precise timestamps** with timezone and microsecond precision
- **Unicode support** (UTF-8 encoding)
- **Message ID field** for event categorization
- **Process ID field** for better process tracking
- **Version field** for protocol evolution
- **Larger message size** support (up to 2048 bytes recommended)

## Message Format Structure

### Complete RFC 5424 Message Format

```
<PRI>VERSION TIMESTAMP HOSTNAME APP-NAME PROCID MSGID [STRUCTURED-DATA] MSG
```

### Visual Breakdown with Brackets

```
[<PRI>][VER] [TIMESTAMP] [HOSTNAME] [APP-NAME] [PROCID] [MSGID] [STRUCTURED-DATA] [MSG]
 │      │    │           │          │         │        │       │                │
 │      │    │           │          │         │        │       │                └─ Free-form message
 │      │    │           │          │         │        │       └──────────────────── Structured metadata
 │      │    │           │          │         │        └──────────────────────────── Message identifier
 │      │    │           │          │         └───────────────────────────────────── Process ID
 │      │    │           │          └─────────────────────────────────────────────── Application name
 │      │    │           └────────────────────────────────────────────────────────── Source hostname
 │      │    └────────────────────────────────────────────────────────────────────── ISO 8601 timestamp
 │      └─────────────────────────────────────────────────────────────────────────── Version (always 1)
 └────────────────────────────────────────────────────────────────────────────────── Priority <0-191>
```

### Detailed Field Breakdown

#### 1. Priority Field `<PRI>`
```
<PRI> = <Facility * 8 + Severity>
Range: <0> to <191>
Same as RFC 3164

Examples:
<16>  = Facility 2 (mail), Severity 0 (emergency)
<34>  = Facility 4 (security), Severity 2 (critical)
<165> = Facility 20 (local4), Severity 5 (notice)
```

#### 2. Version Field
```
Format: Single digit version number
Current: 1 (always "1" for RFC 5424)

Example: 1
```

#### 3. Timestamp Field
```
Format: ISO 8601 with timezone
Full format: YYYY-MM-DDTHH:MM:SS.ssssss+TZ:TZ
Minimum: YYYY-MM-DDTHH:MM:SSZ

Examples:
2025-07-25T13:30:45.123456Z
2025-12-31T23:59:59-05:00
2025-01-01T00:00:00.000001+00:00
- (NILVALUE when timestamp unavailable)
```

#### 4. Hostname Field
```
Format: FQDN, hostname, or IP address of originating system
NILVALUE: - (dash when unavailable)

Examples:
server01.example.com
mail-server
192.168.1.100
-
```

#### 5. App-Name Field
```
Format: Name of the application/process generating the message
NILVALUE: - (dash when unavailable)
Max length: 48 characters

Examples:
sshd
postfix/smtpd
kernel
httpd
myapp
-
```

#### 6. Process ID Field
```
Format: Process ID of the generating process
NILVALUE: - (dash when unavailable)
Max length: 128 characters

Examples:
12345
-
main
worker-01
```

#### 7. Message ID Field
```
Format: Identifier for the type of message
NILVALUE: - (dash when unavailable)
Max length: 32 characters

Examples:
ID47
TCPIN
login-failed
startup-complete
-
```

#### 8. Structured Data Field
```
Format: [SD-ID SD-PARAM="value" SD-PARAM="value"]
NILVALUE: - (dash when no structured data)

Examples:
[exampleSDID@32473 iut="3" eventSource="Application" eventID="1011"]
[timeQuality tzKnown="1" isSynced="1" syncAccuracy="506000"]
-
```

#### 9. Message Field
```
Format: Free-form UTF-8 encoded message
Optional: Can be empty or omitted

Examples:
User admin logged in successfully
Connection from 192.168.1.100 established
System startup completed in 45.2 seconds
```

## Sample RFC 5424 Messages

### System Authentication with Structured Data
```
<38>1 2025-07-25T13:30:45.123456Z server01.example.com sshd 12345 ID47 [auth@32473 user="admin" method="password" result="success"] User admin logged in from 192.168.1.100
```

**Breakdown:**
- `<38>` = Facility 4 (security), Severity 6 (info)
- `1` = Version
- `2025-07-25T13:30:45.123456Z` = ISO 8601 timestamp with microseconds
- `server01.example.com` = Hostname (FQDN)
- `sshd` = Application name
- `12345` = Process ID
- `ID47` = Message ID
- `[auth@32473 ...]` = Structured data with authentication details
- `User admin logged in...` = Message

### Mail System with Timezone
```
<22>1 2025-12-25T14:22:33.456-05:00 mail.example.com postfix/smtpd 8901 CONNECT [origin@32473 ip="203.0.113.45" port="25"] connect from unknown[203.0.113.45]
```

**Breakdown:**
- `<22>` = Facility 2 (mail), Severity 6 (info)
- `1` = Version
- `2025-12-25T14:22:33.456-05:00` = Timestamp with timezone offset
- `mail.example.com` = Hostname
- `postfix/smtpd` = Application name
- `8901` = Process ID
- `CONNECT` = Message ID
- `[origin@32473 ...]` = Structured data with connection details
- `connect from unknown...` = Message

### Kernel Message with NILVALUE Fields
```
<4>1 2025-07-04T08:15:30.789Z firewall kernel - IPTABLES - iptables: DROP IN=eth0 OUT= SRC=10.0.0.100 DST=192.168.1.50
```

**Breakdown:**
- `<4>` = Facility 0 (kernel), Severity 4 (warning)
- `1` = Version
- `2025-07-04T08:15:30.789Z` = Timestamp
- `firewall` = Hostname
- `kernel` = Application name
- `-` = Process ID (NILVALUE)
- `IPTABLES` = Message ID
- `-` = Structured data (NILVALUE)
- `iptables: DROP...` = Message

### Application with Complex Structured Data
```
<134>1 2025-11-30T23:45:12.001+00:00 web01.prod myapp 2468 REQ-PROCESSED [request@32473 id="req-12345" method="GET" path="/api/users" status="200" duration="45.2"][user@32473 id="user-789" role="admin"] Request processed successfully
```

**Breakdown:**
- `<134>` = Facility 16 (local0), Severity 6 (info)
- `1` = Version
- `2025-11-30T23:45:12.001+00:00` = Timestamp with timezone
- `web01.prod` = Hostname
- `myapp` = Application name
- `2468` = Process ID
- `REQ-PROCESSED` = Message ID
- `[request@32473 ...][user@32473 ...]` = Multiple structured data elements
- `Request processed successfully` = Message

## Structured Data Format

### Structure
```
[SD-ID@Enterprise-Number SD-PARAM="value" SD-PARAM="value"]
```

### Components
- **SD-ID**: Structured data identifier
- **Enterprise-Number**: IANA-assigned enterprise number (optional)
- **SD-PARAM**: Parameter name
- **value**: Parameter value (must be quoted)

### Multiple Structured Data Elements
```
[element1@32473 param1="value1"][element2@32473 param2="value2" param3="value3"]
```

### Common Structured Data Elements

#### Time Quality
```
[timeQuality tzKnown="1" isSynced="1" syncAccuracy="506000"]
```

#### Origin Information
```
[origin ip="192.168.1.100" software="MyApp" swVersion="1.2.3"]
```

#### Meta Information
```
[meta sequenceId="1234" sysUpTime="12345678"]
```

## NILVALUE Usage

RFC 5424 uses `-` (dash) as NILVALUE when field data is unavailable:

```
<165>1 2025-07-25T13:30:45Z - myapp - - - Application started
```

Fields with NILVALUE:
- Hostname: `-`
- Process ID: `-`
- Message ID: `-`
- Structured Data: `-`

## Character Encoding and Escaping

### UTF-8 Support
RFC 5424 supports full UTF-8 character encoding in the MSG field:
```
<134>1 2025-07-25T13:30:45Z server01 myapp 1234 INFO - Utilisateur connecté: François
```

### Structured Data Escaping
Special characters in structured data values must be escaped:
- `"` becomes `\"`
- `\` becomes `\\`
- `]` becomes `\]`

Example:
```
[example@32473 message="User said: \"Hello World!\"" path="C:\\Program Files\\MyApp"]
```

## Parsing Considerations

When parsing RFC 5424 messages:

1. **Version Detection**: Check for version field after PRI to identify RFC 5424
2. **Field Separation**: Use space as delimiter, handle NILVALUE (`-`)
3. **Timestamp Parsing**: Parse ISO 8601 format with timezone support
4. **Structured Data**: Parse bracketed elements with proper escaping
5. **UTF-8 Handling**: Support Unicode characters in message field
6. **NILVALUE Handling**: Treat `-` as missing/unavailable data
7. **Multiple SD Elements**: Handle multiple structured data blocks

## Advantages over RFC 3164

1. **Structured Metadata**: Standardized way to include additional data
2. **Precise Timestamps**: Microsecond precision with timezone information
3. **Unicode Support**: Full UTF-8 character set support
4. **Better Identification**: Separate fields for app name, process ID, and message ID
5. **Extensibility**: Structured data allows for future enhancements
6. **Internationalization**: Better support for non-English content

## Common Implementation Variations

- **Timestamp Precision**: Some implementations use milliseconds instead of microseconds
- **Structured Data Usage**: Not all implementations utilize structured data
- **Message Field**: Some omit the message field when structured data is sufficient
- **Enterprise Numbers**: Many implementations omit enterprise numbers in SD-IDs

## Contributing

When adding RFC 5424 samples:

1. **Verify RFC 5424 compliance** including version field and format structure
2. **Include structured data examples** when available
3. **Document timestamp precision** and timezone information
4. **Preserve UTF-8 encoding** for international characters
5. **Explain structured data elements** and their meanings
6. **Note any implementation variations** from the standard

## References

- [RFC 5424 - The Syslog Protocol](https://tools.ietf.org/html/rfc5424)
- [RFC 5424 Section 6 - Syslog Message Format](https://tools.ietf.org/html/rfc5424#section-6)
- [ISO 8601 - Date and Time Format](https://www.iso.org/iso-8601-date-and-time-format.html)
- [IANA Enterprise Numbers](https://www.iana.org/assignments/enterprise-numbers/enterprise-numbers)
- [UTF-8 Character Encoding](https://tools.ietf.org/html/rfc3629)

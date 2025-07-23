# Common Event Format (CEF) Samples

This directory contains log samples in the **Common Event Format (CEF)**. CEF is an open logging standard, originally developed by ArcSight (now part of Micro Focus), that provides a standardized and extensible format for event logs from different sources.

## Understanding CEF and Syslog

It is extremely common to find CEF messages transported using the Syslog protocol. When you encounter a log like this, it's helpful to think of it as two distinct layers: the transport protocol and the message format.

A simple analogy is sending a letter:
* **Syslog** is the **envelope**. It provides the transport metadata, like the Priority (`<PRI>`) header, but doesn't care what's written inside.
* **CEF** is the **letter** itself. It's the structured message payload that contains the actual event details.

### Example Breakdown

Consider this sample log from a Tandem/HP NonStop system:

```log
<134>CEF:0|XYPRO|NONSTOP|XMA|OBJ-ACCESS-PASS|OBJECT-ACCESS-SUCCESS-SFG|4|cs3=FFFE02F360BC9EBC7146...
````

We can break this down into its two main parts:

1.  **The Syslog Header** (defined in RFC 3164 / RFC 5424)

      * This is the `<PRI>` value at the very beginning. It tells the receiving syslog server the message's facility and severity.

    <!-- end list -->

    ```log
    <134>
    ```

2.  **The CEF Message Payload**

      * This is the rest of the string, which strictly follows the CEF standard. It is the rich, structured content of the log.

    <!-- end list -->

    ```log
    CEF:0|XYPRO|NONSTOP|XMA|OBJ-ACCESS-PASS|OBJECT-ACCESS-SUCCESS-SFG|4|cs3=FFFE02F360BC9EBC7146...
    ```

## Samples in This Directory

> [\!NOTE]
> The files in this folder contain CEF-formatted messages. Depending on how the sample was captured, some files may include the syslog `<PRI>` header, while others might contain only the pure CEF message payload.

## Contributing

When adding new CEF samples, please ensure they are properly sanitized. For full guidelines on naming and contribution, please see the main [`CONTRIBUTING.md` file](https://github.com/sva-s1/log-zoo/blob/main/CONTRIBUTING.md) in the root of the repository.

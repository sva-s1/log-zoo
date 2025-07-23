# 🪵 log-zoo

A community-driven repository of raw, unparsed log samples from various vendors and applications.

[![CC0](https://licensebuttons.net/p/zero/1.0/88x31.png)](https://creativecommons.org/publicdomain/zero/1.0/)

## About This Project

The goal of `log-zoo` is to provide a centralized collection of real-world log examples for development, testing, and security research. These samples are useful for building and validating parsers, testing normalization rules, and understanding data structures from different log sources.

> [!IMPORTANT]
> The logs in this repository are **raw events** sourced directly from vendors (e.g., cloud applications, network appliances, self-hosted software). They have **not** been parsed or processed by SentinelOne's Security DataLake (SDL) or any other SIEM.

***

## Directory Structure

Log samples are organized by their format or transport protocol at the top level. The file names should be descriptive of the vendor and event type.

```

.
├── cef/
├── csv/
├── json/
├── leef/
└── syslog/
├── rfc3164/
└── rfc5424/

````

* **`cef/`**: ArcSight Common Event Format (CEF) logs.
* **`csv/`**: Comma-separated value logs.
* **`json/`**: Logs in JSON format, either as single objects or one object per line.
* **`leef/`**: IBM Log Event Extended Format (LEEF) logs.
* **`syslog/`**: Contains logs transported via syslog protocol, separated by their RFC standard.
    * `rfc3164/`: The legacy BSD-style syslog protocol.
    * `rfc5424/`: The newer, standardized syslog protocol.

> [!NOTE]
> While formats like **CEF** and **LEEF** are often transported via syslog, they are distinct log formats. They have their own top-level directories to accommodate samples that may be sourced from files (`.log`, `.txt`) instead of a direct syslog stream.

***

## How to Use

Clone the repository to get local access to the samples. You can use these files as test data for scripts, parser development, or any other analysis.

```bash
git clone [https://github.com/your-org/log-zoo.git](https://github.com/your-org/log-zoo.git)
````

-----

## Additional Resources 🛠️

For pre-built dashboards, monitors, parsers, and other configurations that can be used with your log data, please visit the official Scalyr samples repository. While this `log-zoo` repo provides the **raw** data, the `scalyr/samples` repo provides assets to help you process and visualize it.

  * **Visit:** **[github.com/scalyr/samples/](https://github.com/scalyr/samples/)**

-----

## License

This repository is dedicated to the public domain under the [CC0 1.0 Universal](https://www.google.com/search?q=LICENSE) license. You can copy, modify, and distribute the work, even for commercial purposes, without asking permission.


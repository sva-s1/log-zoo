# Contributing to log-zoo

Thank you for your interest in contributing! Your help is essential for making this repository a valuable resource for everyone. By contributing, you agree to release your work into the public domain under the [CC0 1.0 Universal](LICENSE) license.

---

## The Golden Rule: Sanitize Everything!

Before you commit any file, you must remove all sensitive information. This is the most important rule.

> [!CAUTION]
> **All personally identifiable information (PII) and customer-specific data MUST be removed.** Failure to sanitize data will result in your pull request being rejected.

Scrub your log samples for:
* Usernames, email addresses, and real names
* Internal and external IP addresses (replace with private ranges like `10.0.0.0/8` or `192.168.0.0/16`)
* Hostnames, server names, and domain names
* API keys, tokens, passwords, and other secrets
* Company or customer-specific identifiers

When in doubt, replace it with a generic placeholder like `[REDACTED_HOSTNAME]` or `user@example.com`.

---

## How to Contribute

1.  **Fork the repository** to your own GitHub account.
2.  **Create a new branch** for your changes. Please name it descriptively (e.g., `add-cisco-asa-logs`).
    ```bash
    git checkout -b add-cisco-asa-logs
    ```
3.  **Add your sanitized log files** to the appropriate directory. If a suitable directory doesn't exist, feel free to create one.
4.  **Commit your changes** with a clear commit message.
5.  **Open a Pull Request (PR)** to the `main` branch of the upstream repository.

---

## Pull Request Guidelines

* **Use a descriptive PR title**, such as "feat: Add Fortinet FortiGate CEF logs" or "fix: Sanitize IPs in Palo Alto samples".
* **In the PR description**, please provide context about the log source. Include:
    * The vendor and product (e.g., "Microsoft Defender for Endpoint").
    * The format of the log (e.g., "JSON").
    * Any other relevant details that might help users understand the data.

We'll review your contribution and merge it as soon as possible. Thank you for helping build the zoo!

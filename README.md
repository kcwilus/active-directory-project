# Active Directory & Splunk Security Monitoring Lab

A hands-on home lab built to practice Windows domain administration, endpoint log collection, SIEM searches, and investigation of failed logons and RDP password-guessing activity in an isolated virtual environment.

> **Scope note:** The tutorial's Atomic Red Team exercise was not completed and is intentionally excluded. This repository documents only the lab work and results supported by the available screenshots. No credentials are published.

## Lab overview

The lab combines a Windows domain environment with Splunk for security event analysis and Kali Linux for authorized testing in the lab.

### Technologies observed in the lab

- Windows Server / Active Directory
- Windows endpoint
- Splunk Enterprise
- Kali Linux
- Hydra (RDP module)
- VirtualBox (virtualized lab environment)

Exact product versions and full topology have not yet been recorded in this repository.

## Verified results

### 1. Failed logon events in Splunk

A Splunk search was run against the `endpoint` index for Windows Security Event ID 4625:

```spl
index="endpoint" mb EventCode=4625
```

The screenshot shows **29 events** in the selected seven-day time range. The visible events identify:

- **Event ID:** 4625 — failed logon
- **Host:** `target-PC`
- **Source:** `WinEventLog:Security`
- **Sourcetype:** `WinEventLog:Security`

This confirms that failed Windows Security logon events were searchable in Splunk at the time of the screenshot.

### 2. RDP password-guessing test from Kali Linux

The terminal screenshot shows Hydra running against the lab target's RDP service:

- **Target IP:** `192.168.10.100`
- **Target port:** `3389/tcp` (RDP)
- **Username tested:** `mb`
- **Password list attempts shown:** 24
- **Observed result:** Hydra reported one valid password for the test account.

The password itself is deliberately **not included** in this repository. Do not publish it in screenshots, notes, command history, or commits. If this credential is still active, change it before sharing the lab publicly.

This was a controlled lab test. Do not run password-guessing tools against systems without explicit authorization.

### 3. Relationship between the test and SIEM events

The Splunk screenshot shows 29 events matching Event ID 4625, while the Hydra screenshot shows an RDP guessing attempt. These screenshots demonstrate relevant lab activity, but by themselves they do **not** prove that all 24 Hydra attempts generated the 29 Splunk events or that the events were correlated to the exact test. That conclusion would require matching timestamps, source/target fields, and event details.

## Architecture

Conceptually, the lab includes a Windows domain environment, an endpoint producing security events, Splunk for searching those events, and Kali Linux as the testing machine.

The exact hostnames, domain name, IP assignments for every machine, network mode, log-forwarding configuration, and Sysmon settings are not confirmed by the currently supplied evidence. They should be added only after verification.

## Screenshots

Add your own redacted screenshots to a `screenshots/` directory. Suggested evidence:

1. Splunk search showing Event ID 4625 results.
2. Hydra terminal output with the discovered password fully hidden.
3. Active Directory/domain configuration.
4. Verified log collection and any additional searches you performed.

Before committing screenshots, hide passwords, tokens, personal data, and any network details you do not want public. The screenshot of Hydra must not expose the recovered password.

## Reproducibility notes

Record the actual environment settings before documenting exact reproduction steps:

- Windows Server and Windows client versions
- Domain name and controller/client hostnames
- Private lab IP addresses and VirtualBox network mode
- Splunk version, index, sourcetype, and event collection method
- Sysmon version and configuration, if used
- Any searches or detections that were personally tested

Do not treat unknown settings as confirmed facts.

## Safety and limitations

- Use these techniques only in a lab you own or are explicitly authorized to test.
- Keep test systems isolated from production and personal networks.
- Never commit passwords, tokens, private keys, or unredacted sensitive logs.
- Atomic Red Team was not completed and is outside the scope of this project.
- This is a learning lab, not a claim of production-ready detection coverage.

## Credits

Created as a personal learning project based on an Active Directory and security monitoring tutorial series. Product names are the property of their respective owners.

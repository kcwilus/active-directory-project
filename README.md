# Active Directory & Splunk Security Monitoring Lab

A hands-on cybersecurity lab built while following a five-part Active Directory and security-monitoring tutorial series. The project combines a Windows domain environment, endpoint telemetry, Splunk searches, and a controlled RDP authentication test from Kali Linux.

> **Scope:** The Atomic Red Team exercise at the end of the series was not completed and is intentionally excluded. This repository documents observed results and configuration details supplied for this lab. Unknown values are marked as unconfirmed rather than guessed.

## Objectives

- Build a small Windows Active Directory environment in VirtualBox.
- Practice domain and Windows endpoint administration.
- Collect and search Windows Security events in Splunk.
- Use Sysmon telemetry as part of endpoint monitoring.
- Observe failed logon activity and review relevant SIEM events.
- Document the lab architecture and evidence for a cybersecurity portfolio.

## Lab components

| Component | Address / details | Purpose |
|---|---|---|
| Windows Server | `192.168.10.101`; gateway reported as `192.168.10.1` | Windows Server and Active Directory domain services |
| Windows 10 Pro target | `192.168.10.100` | Windows endpoint used for the lab and RDP test |
| Splunk Enterprise | `192.168.10.10` (reported as likely; not independently verified) | Searching and reviewing Windows events |
| Kali Linux | `192.168.10.250` (reported; not independently verified) | Lab testing workstation |
| VirtualBox | NAT networking reported for all VMs | Virtualization |

**Networking note:** The addresses above are the values supplied by the project author. The Splunk and Kali addresses are not confirmed by the supplied screenshots. The precise VirtualBox NAT arrangement and inter-VM routing have not been independently verified.

## Active Directory

- **Domain:** `kcwilus.local`
- **User accounts reported:** `mb` and `mk`
- **Domain controller hostname:** not confirmed in the supplied details.
- **Organizational units, group membership, and domain functional level:** not recorded yet.

Avoid committing passwords or other account secrets. User display names are omitted from this public overview.

## Verified results

### 1. Failed logon events in Splunk

The supplied Splunk screenshot shows this search:

```spl
index="endpoint" mb EventCode=4625
```

Observed in the screenshot:

- **Result count:** 29 events
- **Selected time range:** Last 7 days
- **Event ID:** 4625 — failed logon
- **Host:** `target-PC`
- **Source:** `WinEventLog:Security`
- **Sourcetype:** `WinEventLog:Security`
- **Visible account name in event details:** `mb`

This confirms that events matching the search were available in Splunk at the time of the screenshot. The screenshot does not establish the complete log-forwarding configuration or prove that every event came from the RDP test below.

### 2. RDP authentication test from Kali Linux

The supplied terminal screenshot shows Hydra v9.6 testing the lab RDP service:

- **Target:** `192.168.10.100:3389/tcp`
- **Service:** RDP
- **Username tested:** `mb`
- **Attempts shown:** 24
- **Observed result:** Hydra reported one valid password for the test account.

The recovered password is intentionally not documented. Do not publish it in this repository or in screenshots. If it remains active, change it before making the project public.

This activity was performed in the author's lab. Only test systems you own or are explicitly authorized to assess.

### 3. Correlation limitations

The Splunk screenshot shows 29 matching Event ID 4625 events, and the Kali screenshot shows an RDP password-guessing test. The available screenshots alone do not prove a one-to-one relationship between the Hydra attempts and the Splunk events. To validate correlation, inspect timestamps and fields such as source address, destination host, account, and logon type in the actual events.

## Sysmon and event collection

The project author reports using a Sysmon configuration associated with an author named Olaf on GitHub. The exact repository URL, configuration revision, Sysmon version, and event-forwarding method have not been confirmed in the supplied details.

Document those values here once verified. Do not claim that a particular Sysmon event or detection was observed unless supported by a screenshot or saved configuration.

## Tutorial materials

The project follows the tutorial series linked below. The five supplied videos are listed for reference; the Atomic Red Team exercise is out of scope.

1. [Tutorial video — part 1](https://www.youtube.com/watch?v=mWqYyl89QaY&list=PLGrcVHQv6mp-_5jY1XU1SJ3tjoUMrTI7E&index=2)
2. [Tutorial video — part 2](https://www.youtube.com/watch?v=2cEj3bS5C0Q&list=PLGrcVHQv6mp-_5jY1XU1SJ3tjoUMrTI7E&index=3)
3. [Tutorial video — part 3](https://www.youtube.com/watch?v=uXRxoPKX65Q&list=PLGrcVHQv6mp-_5jY1XU1SJ3tjoUMrTI7E&index=4)
4. [Tutorial video — part 4](https://www.youtube.com/watch?v=1XeDht_B-bA&list=PLGrcVHQv6mp-_5jY1XU1SJ3tjoUMrTI7E&index=6)
5. [Tutorial video — part 5](https://www.youtube.com/watch?v=orq-OPIdV9M&list=PLGrcVHQv6mp-_5jY1XU1SJ3tjoUMrTI7E&index=19)

The links are recorded as supplied by the author. Exact titles and ordering should be checked against the playlist before publication if they are to be presented as official episode titles.

## Screenshots and evidence

The project author will add screenshots manually. Before committing them, redact passwords, tokens, personal information, and any other details that should not be public. In particular, make sure the recovered RDP password is completely obscured.

Recommended evidence to add:

- Splunk search results for Event ID 4625.
- Redacted terminal output from the RDP test.
- Active Directory domain and user configuration.
- Sysmon configuration and verified event-collection settings.
- Additional Splunk searches or dashboards actually tested.

## Values still to verify

- Exact versions of Windows Server, Windows 10, Splunk, Sysmon, Kali Linux, and VirtualBox.
- Domain controller hostname and full network configuration.
- Whether `192.168.10.10` is the actual Splunk IP and `192.168.10.250` the actual Kali IP.
- Exact Sysmon configuration source URL and revision.
- Splunk event collection/forwarding method and configuration.
- Whether the failed logons shown in Splunk correspond to the RDP test.

## Safety and limitations

- Use the lab only for authorized testing.
- Keep security-testing systems isolated from production and personal networks.
- Never commit passwords, tokens, private keys, or unredacted logs.
- Atomic Red Team was not completed and is intentionally not included.
- This is a learning project, not a claim of production-ready detection coverage.

## Credits

Personal learning project based on the linked tutorial series. Product and tool names belong to their respective owners.

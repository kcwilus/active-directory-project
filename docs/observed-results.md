# Observed Lab Results

This page records the results visible in the screenshots supplied for the project. It intentionally excludes the recovered password.

## Splunk: failed logon events

Search shown in the screenshot:

```spl
index="endpoint" mb EventCode=4625
```

| Field | Observed value |
|---|---|
| Result count | 29 events |
| Time range | Last 7 days |
| Event code | 4625 |
| Host | `target-PC` |
| Source | `WinEventLog:Security` |
| Sourcetype | `WinEventLog:Security` |
| Visible account name | `mb` |

The screenshot confirms that events matching the query were searchable in Splunk at that time.

## Kali Linux: RDP test

The terminal screenshot shows Hydra v9.6 targeting the lab RDP service:

```bash
hydra -l mb -P passwords.txt rdp://192.168.10.100 -s 3389 -V -t 1
```

Observed details:

- Target: `192.168.10.100:3389/tcp`
- Username: `mb`
- Password attempts shown: 24
- Result: Hydra reported one valid password.

The recovered password is intentionally omitted. If it is still active, change it. Do not commit the password or unredacted screenshots.

## Network values reported by the project author

| Component | Value | Confidence |
|---|---|---|
| Windows Server | `192.168.10.101` | Supplied by author |
| Default gateway | `192.168.10.1` | Supplied by author |
| Windows 10 Pro target | `192.168.10.100` | Visible in Hydra command |
| Splunk | `192.168.10.10` | Lab configuration supplied by the author |
| Kali Linux | `192.168.10.250` | Lab configuration supplied by the author |
| Active Directory domain | `kcwilus.local` | Supplied by author |

The lab configuration uses NAT networking for all VMs, as supplied by the project author.

## Correlation caveat

The Splunk screenshot shows 29 failed-logon events and the terminal screenshot shows an RDP password-guessing test. The available evidence does not prove that these are the same activity. Verify timestamps and event fields such as source address, destination host, account, and logon type before asserting correlation.

## Sysmon

The author reports using a Sysmon configuration associated with “Olaf” on GitHub. The exact URL, revision, version, and forwarding configuration are not yet known. Do not invent these details.

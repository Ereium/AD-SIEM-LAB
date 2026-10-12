# AD-SIEM-LAB
A Siem Lab that was configured from my other project: https://github.com/Ereium/Active-Directory-Homelab

Active Directory & Splunk Telemetry Lab
A home lab I set up to practice Active Directory security monitoring, log forwarding, and threat detection using Splunk.

Environment Setup
Domain Controller: DC01 running Windows Server 2022 (lab.local)

SIEM: Splunk Enterprise on host (10.0.0.124) listening on port 9997

Log Collector: Splunk Universal Forwarder pulling logs via inputs.conf

Endpoint Logging: Sysmon v15.22 using SwiftOnSecurity's profile to capture process creations, network connections, and thread activity

Auditing & Telemetry
Windows Server doesn't log every Kerberos detail out of the box. To capture Kerberos Service Ticket requests (Event ID 4769), I had to manually enable subcategory auditing for Kerberos Service Ticket Operations using auditpol.

Testing & Detections
1. Simulating Kerberos Requests
To test if Kerberos ticket activity was actually getting picked up by Splunk, I used PowerShell to request a ticket for a test Service Principal Name (SPN).

2. Splunk SPL & Field Extraction
The default table view in Splunk left several XML fields empty because raw Windows security logs don't automatically parse custom fields. I wrote an inline regex (rex) query in SPL to extract the TargetAccountName and TargetServiceName directly from the raw event body into a clean table.

Quick Lessons Learned
Server 2022 Defaults: Modern Windows Server builds default to strong AES encryption (0x18 or 0x12) instead of legacy RC4 (0x17), making default Kerberoasting harder without configuration changes.

Log Parsing: Standard Windows event fields are often buried inside unparsed XML, so writing custom regex directly inside SPL searches is usually necessary to get readable reports.

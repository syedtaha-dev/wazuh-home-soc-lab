# wazuh-home-soc-lab
Wazuh Home SOC Lab

A hands-on Security Information and Event Management (SIEM) lab built using Wazuh, deployed across two physical machines on a home network. This project simulates a real-world SOC monitoring setup: a central manager collecting and analyzing security events from a remote endpoint.

Objective

To gain practical, hands-on experience with SIEM deployment, log collection, detection engineering, and alert triage — skills directly applicable to a SOC Analyst role.

Architecture
┌─────────────────────────┐         LAN (192.168.1.0/24)        ┌──────────────────────────┐
│   Desktop (Parrot OS)   │◄───────────────────────────────────►│  Laptop (Windows 11 Pro) │
│   192.168.1.14          │                                     │  192.168.1.9             │
│                         │                                     │                          │
│   - Wazuh Manager       │                                     │   - Wazuh Agent          │
│   - Wazuh Indexer       │              agent reports events   │   - Sends Windows event  │
│   - Wazuh Dashboard     │◄────────────────────────────────────│     logs to manager      |
└─────────────────────────┘                                     └──────────────────────────┘
Manager host: Parrot Security OS (Debian-based), running Wazuh Indexer + Manager + Dashboard (all-in-one install)
Monitored endpoint: Windows 11 Pro laptop running the Wazuh agent
Both machines communicate over a local home network (LAN + WiFi, same subnet)
Setup Summary
Installed Wazuh 4.14 (indexer, manager, dashboard) on Parrot OS using the official all-in-one installer script
Verified all three services running via systemctl status
Retrieved auto-generated admin credentials and logged into the Wazuh Dashboard
Deployed the Wazuh agent on a Windows 11 laptop via the dashboard's "Deploy new agent" wizard, using a generated PowerShell/MSI install command
Confirmed agent registration and active connection in the dashboard, including live system inventory (CPU, RAM, hostname) pulled from the endpoint
Simulated failed login attempts on the Windows endpoint to generate real detection events
Verified alerts appeared in the Threat Hunting module
Detection Example: Failed Logon Attempts

Simulated 4 failed login attempts on the Windows endpoint. Wazuh detected and alerted on all 4 within seconds.

Field	Value
Rule ID	60122
Rule Description	Logon Failure - Unknown user or bad password
Rule Level	5
Agent	taha_hp
Detection Window	~4 seconds (4 attempts)

Analyst interpretation: Multiple failed logons in a short window from a single host is an early-stage indicator consistent with brute-force login attempts. While a single low-severity alert like this isn't an incident on its own, it's exactly the kind of signal a SOC analyst would correlate with other events (e.g., a successful logon immediately after, or a higher volume of attempts) to identify a potential account compromise attempt. This maps conceptually to MITRE ATT&CK T1110 (Brute Force).


Skills Demonstrated
SIEM deployment and configuration (Wazuh indexer, manager, dashboard)
Cross-platform agent deployment (Linux manager, Windows endpoint)
Network/LAN troubleshooting for agent-manager connectivity
Log analysis and alert triage using Wazuh's Threat Hunting module
Understanding of detection rules, severity scoring, and MITRE ATT&CK mapping
Next Steps
Add a second endpoint (vulnerable Linux VM) to broaden log source diversity
Simulate additional attack types: port scanning (Nmap), brute-force escalation, malware execution
Write custom detection rules beyond Wazuh's defaults
Integrate with a threat intelligence feed for IOC enrichment
Screenshots


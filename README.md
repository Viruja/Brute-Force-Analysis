# Brute-Force-Analysis
## Windows Event Logs Analysis in the event of a RDP Brute Force

### Overview
* Objective : Detection and Analyzing of a RDP Brute Force Attack using Windows Events Logs
* Data Source : Generated .evtx files from self-simulation of a RDP Brute Force
* Tools used : Event Viewer, TimeLine Explorer

### Scenario Description
* A self-simulated RDP brute force attack where an attacker repeatedly attempted RDP logins using multiple passwords on a Windows host
* In the scenario the attacker tried to gain remote access to the administrator account of the Windows host.
* Attacker (192.168.0.106) -> Target (192.168.0.103)

### Log Files Used
* Security.evtx

### Methodology
Step 1 : Load logs
  * Opened Event Viewer to analyze Windows Logs
Step 2 : Filtering
  * Moved to the Security logs under Windows logs
  * Filtered the created logs to reduce noise by using the *Filter* option and applied filteration by Event-IDs.
    - **4624** - Successful Login
    - **4625** - Failed Login
Step 3 : Analysis
  * Source Network Address : 192.168.0.106

### Key Observations
  * Multiple 4625 Event IDs (failed logons) from 192.168.0.106 were observed, followed by a 4624 Event ID (successful logon) from the same IP.
  * Here, the RDP logons primarily generated Event ID 4625 with Logon Type 3. This is normal when password guessing happens before the establishment of RDP access (NLA stage - Network Level Authentication).
  * If NLA is disabled and the attacker reached the RDP logon screen, the failed attempts will be recorded as Logons Type 10.

### Detections
  * Monitor Windows Security logs for 4625 ID events from a same IP address in a short span of time.

### Mitigation
  * Restrict RDP Access to trusted hosts or through VPN only
  * Enable multi-factor authentication

### Screenshots
Event ID 4625 (Failed Attempts)<img width="896" height="212" alt="Event ID 4625 (Failed Attempts)" src="https://github.com/user-attachments/assets/afbaec7c-983b-4769-8438-5cfdc999f319" />
<br><br>
Event ID 4624 (Successful Attempt). The EventID 4672 indicates that the account which the attacker brute forced and gained access has administrative privileges.<img width="1366" height="464" alt="Event ID 4624 (Successful Attempt)" src="https://github.com/user-attachments/assets/3d1d522b-c80c-44c4-97e9-8981dea50847" />
<br><br>
Detailed view from Event Viewer which indicates the attacking machine's IP Address.<img width="1366" height="727" alt="Detailed view from Event Viewer" src="https://github.com/user-attachments/assets/57a37013-0345-496e-833d-4d11c58311e6" />



### References
* Windows Event Cheat Sheet - https://www.malwarearchaeology.com/cheat-sheets
* Windows Events IDs - https://learn.microsoft.com/en-us/windows-server/identity/ad-ds/plan/appendix-l--events-to-monitor

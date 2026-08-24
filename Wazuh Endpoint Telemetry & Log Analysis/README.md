<div align="center">

# **Objective**
  
</div>

<div align="center">
Configure Wazuh to collect, filter, and analyze endpoint security events from both Windows and Linux systems. This project demonstrates how to simulate user creation, privilege escalation, and SSH logins, then trace those actions in Wazuh using raw log artifacts, Event IDs, SIDs, and session IDs.
</div>
<div align="center">

# **Tools & Technologies**
  
</div>

- Wazuh SIEM & Dashboard: Log ingestion, agent management, and DQL query filtering.

- Windows OS: Command Prompt (net user commands), Windows Event Log (Security.evtx).

- Linux (Ubuntu): sshd, systemd-logind, Linux Sysmon, shell logging.

- Analysis Techniques: Windows Event ID mapping, SID/RID parsing, UAC bitmask calculation.

<div align="center">
  
# **Skills Learned**

</div>

**Wazuh Agent Configuration:** Edited ossec.conf to pull logs from the Windows Security event channel while filtering out noisy Event IDs directly at the agent level.

**Windows Threat Hunting & Forensic Analysis:**

Tracked user creation, local group modification (Event ID 4732), and account deletion (Event ID 4726).

Resolved missing username fields by searching target SIDs and RIDs across historical log entries.

Decoded User Account Control (UAC) hex values (e.g., 0x15 = 0x10 + 0x4 + 0x1) to verify account status flags like disabled accounts or bypass flags.

**Linux SSH & Session Tracking:**

Monitored invalid user login attempts and successful authentication in /var/log/auth.log.

Correlated SSH session start and end times by linking session scope identifiers (e.g., session-20.scope) with matching sshd process IDs.

<div align=center>
  
# SOC-Lab-and-Incident-Investigation

</div>

### **Project Objective**
The main goal of this project was to deploy a fully functional, isolated security monitoring environment. I wanted to simulate real world attacks (such as credential dumping and SSH brute-forcing), analyze the resulting telemetry, write custom rules to detect these threats, and configure automated host isolation to stop them in their tracks.

### **Tools & Technologies Used**

- **SIEM / XDR:** Wazuh (Manager & Agents)

- **Endpoint Telemetry:** Microsoft Sysmon (Windows) & Sysmon for Linux

- **Hypervisor:** VMware Workstation Pro

- **Operating Systems:** Ubuntu Server 24.04 LTS (SIEM & Linux Agent) & Windows 11 Enterprise

- **Defensive Controls:** Linux IPTables (Firewall) & Wazuh Active Response

### **Core Skills Learned**

- **SIEM Administration:** Deploying a central manager, enrolling Linux/Windows agents, and modifying agent-side configurations (ossec.conf) to stream custom event channels.

- **Telemetry Configuration:** Customizing Sysmon templates (such as Olaf Hartong’s configurations) to filter system noise and capture critical security logs (like process creations and network connections).

- **Detection Engineering:** Authoring custom XML detection rules in Wazuh using case-insensitive PCRE2 regular expressions (specifically targeting metadata like OriginalFileName to catch renamed execution files).

- **Automated Threat Mitigation:** Implementing Wazuh Active Response to dynamically invoke scripts (firewall-drop) and automatically insert blocking rules into the local firewall (iptables).

- **File Integrity Monitoring (FIM):** Configuring real-time folder monitoring on both Windows and Linux endpoints to audit unauthorized modifications or deletions of critical files.

- **Forensic Investigation:** Parsing raw JSON security archives (wazuh-archives-*) in the SIEM dashboard, tracing Security Identifiers (SIDs/RIDs), and building a chronological forensic incident timeline.

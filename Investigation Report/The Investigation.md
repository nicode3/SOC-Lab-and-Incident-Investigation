<div align=center>
  
# Security Operations Center (SOC) Lab and Incident Investigation Report: Wazuh SIEM/XDR Deployment 

</div>

**Overview:**

In the modern threat landscape, the strategic deployment of a controlled SIEM (Security Information and Event Management) and XDR (Extended Detection and Response) environment is an absolute necessity for security operations. This lab serves as a high-fidelity environment for testing the detection of advanced adversary techniques. By integrating Wazuh with Sysmon, we achieve a level of granular visibility that standard logging cannot provide, enabling the collection of deep telemetry required to detect indicators of compromise (IOCs) before they manifest into catastrophic breaches. 

**Technical Infrastructure Summary:**

The infrastructure is built upon a multi-node architecture designed to simulate a standard corporate environment utilizing both Windows and Linux assets.

**Wazuh Manager | Ubuntu 24.04 | Centralized SIEM/XDR Engine | 8GB RAM, 4 CPU Cores**

**Windows Agent | Windows 11 | Endpoint Workstation | 8GB RAM, 2 CPU Cores**

**Linux Agent | Ubuntu Server 24.04 | Application/Server Asset | 4GB RAM, 2 CPU Cores**

**Architectural Mechanics:**

The Wazuh Agent serves as the "eyes and ears" of the SOC. These lightweight agents are deployed to endpoints to provide continuous monitoring, forwarding security telemetry to the central Wazuh Manager. This agent-manager communication is centralized over Port 1515 for agent enrollment/management and Port 1514 for secure event data transmission.

<div align=center>
 
## **Security Alert Finding #1**

</div>

- By default the Windows Guest Account is disabled. Attackers often try to enable it to create backdoor persistence.

- I engineered a custom Wazuh rule to detect Event ID 4722 indicating that the account was enabled.

- I extracted the specific log fields to build a highly granular search query: data.win.eventdata.targetUserName: Guest and data.win.system.eventID: 4722 

In this simulated environment where I shut down a self generating attack by creating a custom rule that stops the attacker from gaining persistence through enabling guest accounts. To narrow down on the log I needed for this attack I created a custom rule and a search query to narrow down the finding to the logs exactly correlated to the attack. 


**Who** | Simulated SOC Environment. Self Generated Logs. No Outside Involvement.

**What** | An unauthorized Guest Account was enabled in a Windows 11 machine 

**When** | This occurred on August 19 2026 @ 15:58:40 , it was logged 16:15:30

**Where** | Inside the Simulated SOC network on the Windows 11 Endpoint (Wazuh Agent)

**Why** | To test our custom Wazuh rule to detect account enabling (4722)

**How** | Command “user guest /active:yes” ran on Windows 11 cmd


**Recommendation:**

- The Guest account is disabled by default for a reason. Instead of just watching it get enabled, the best defense is to use a Group Policy Object (GPO) to permanently lock the Guest account status to Disabled across all endpoints. This prevents anyone from easily toggling it on without extra authorization steps. 

- This exercise highlighted how quickly threat actors can move once they establish persistence. Instead of waiting for a manual analyst review, a key takeaway is configuring the SIEM's Active Response engine to automatically trigger a script that disables the Guest account the millisecond it is enabled, neutralizing the threat instantly. 

- While Windows Security Log Event ID 4722 alerted me that the Guest account was enabled, it didn't show how it was done. Only Sysmon Event ID 1 (Process Creation) captured the exact command line (net user guest /active:yes) and identified the process that executed it. Relying on just one log source leaves a blind spot. 

<div align=center>
 
## **Security Alert Finding #2**

</div>

- Failed SSH connection attempt on the Ubuntu agent generated a raw "failed password" log 

- Rather than relying on generic alerts, a custom detection rule was authored to trigger a high-severity alert only when 3 failed SSH login attempts occur from the same source IP within a 120-second window 

- These failed SSH events are fed into a dedicated Data Table on the Wazuh dashboard to track the attacker's timestamp, source IP, and targeted username 

I set up Wazuh Active Response to block SSH brute-force attacks by triggering a firewall-drop script after 3 failed logins in 120 seconds. This dynamically blocks the attacker's IP using local Linux iptables, which I validated in real-time when my continuous test ping immediately timed out 

**Who** | Simulated SOC Environment. Self Generated Logs. No Outside Involvement.

**What** | This defense mechanism leverages raw SSH "failed password" logs to trigger a custom Wazuh threat-detection rule. Upon triggering, the manager executes a firewall-drop active response script to block the source IP 

**When** | Logs Generated on August 19, 2026 @ 22:24:25

**Where** | Ubuntu Server 24.04 LTS 

**Why** | The system was engineered to transition the environment from passive alerting to automated, host-level threat mitigation. This completely shuts down brute-force attacks in real time without requiring human analyst intervention 

**How** | I authored a custom rule to detect SSH failures, configured the firewall-drop active response script, and validated the setup by simulating a brute-force attack. This resulted in an active network ping instantly timing out as Wazuh dynamically dropped the attacker's IP, which I later manually cleared to restore connectivity 


**Recommendations:**

- Blocking a user after only three wrong passwords can easily lock out real workers by mistake. We should raise the login attempt limit or make a list of safe computer IP addresses that never get blocked.
 
- We can utilize the Dashboard tables to see which IP addresses keep trying to break in. This helps us spot persistent attackers and block them permanently from the network.

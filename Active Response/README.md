<div align=center>

# **Automated Threat Mitigation (Active Response)**

</div>

I configured my SIEM to go beyond passive alerting and take active, automated defensive action. I set up the environment so that when Wazuh detects repeated failed SSH login attempts on my Ubuntu server, it automatically blocks the attacker's IP at the firewall level.

### **Simulating the Brute-Force Attack:**

- To test how the SIEM tracks authentication failures, I initiated an SSH session from my Windows terminal to my Ubuntu server and intentionally failed the password three times.

- In my Wazuh Manager's local_rules.xml, I authored custom rule 100101. This rule aggregates PAM and SSH failures, triggering a high-priority alert if 3 failed SSH logins are observed from the same source IP within a 120-second window.

### **Configuring the Active Response Script:**

- I opened the Wazuh Manager's configuration file (/var/ossec/etc/ossec.conf) on my Ubuntu server.

- Enabled and configured an <active-response> block to trigger the default firewall-drop script.

- Configured the block with <location>local</location> so that the block executes directly on the Ubuntu endpoint target, and tied it directly to my custom brute-force rule ID 100101.

- Restarted the manager and verified that the firewall-drop script was successfully loaded by querying Wazuh's command-line agent control utilities.

### **Testing Automated Isolation (Proof of Concept):**

- To monitor network connectivity in real-time, I started a continuous ping (ping -t) from my Windows VM to my Ubuntu server.
In a separate terminal window, I simulated a second SSH brute-force attack.

- **The Result:** On the third failed login attempt, my custom rule fired. 

- Within milliseconds, the active response engine kicked in, and the live ping terminal on my Windows machine instantly changed to Request timed out. 

- The attacker's terminal hung, confirming total host isolation.

- Checking the Wazuh Dashboard, I verified a success event: **"host blocked by firewall-drop active response".**

### **Analyst Investigation & Manual Overrides:**

- To audit what happened under the hood, I logged into my Ubuntu server and inspected the local Linux firewall tables.

- I ran sudo iptables -L -n --line-numbers and confirmed that Wazuh had dynamically injected DROP rules containing my Windows host's IP address into the active INPUT and FORWARD firewall chains.

- To manually restore my connection before the automatic 10-minute timeout expired, I cleared the firewall entries by running sudo iptables -D INPUT 1 and sudo iptables -D FORWARD 1.

- The Windows terminal's continuous ping immediately resumed receiving replies, validating the recovery process

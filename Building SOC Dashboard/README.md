<div align=center>
  
## **The Three-Panel SOC Dashboard**

</div>

**To monitor and visualize security events in real-time, I built a custom, three panel dashboard within the Wazuh SIEM:**

### **Failed Windows Logins (Metric Panel)**

**What it is:** A single, large number that instantly shows the count of failed Windows login attempts

**How it works:** It filters raw telemetry logs for Windows Event ID 4625 (the standard event code for a failed logon) using the field data.win.system.eventID


### **Windows Account Changes Over Time (Line Chart)**

**What it is:** A line graph that tracks user account creations, modifications, and deletions to help spot unauthorized local changes.

**How it works:** It queries security event IDs like 4720 (user created) and 4726 (user deleted). It plots these over a timeline and splits the lines by Event ID so you can see spikes in specific actions.


### **Linux Failed SSH Logins (Data Table)**

**What it is:** A structured table that audits failed password attempts on the Linux server.

**How it works:** It searches for "failed password" logs on the Linux endpoint and breaks the data into rows containing the Timestamp, Source IP, and Source User so you can quickly see who is trying to brute-force their way in

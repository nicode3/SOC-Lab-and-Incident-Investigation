<div align=center>
  
# **Real-Time File Integrity Monitoring (FIM)**
  
</div>

Configuring real-time File Integrity Monitoring (FIM) on both my Windows and Linux target endpoints to instantly alert my central SIEM whenever critical system folders are modified or deleted.

### **Windows Setup:**

I created a directory at C:\CompanyData and added a mock confidential file named payroll.txt with a dummy sensitive message.
I modified the local Wazuh agent's configuration file (ossec.conf) to set <directories realtime="yes">C:\CompanyData</directories> and restarted the Wazuh service to apply the changes.

**Testing:** Appending "123" to payroll.txt instantly generated an "integrity check sum changed" alert in my Wazuh dashboard events. Deleting the file immediately generated a "file deleted" alert.

### **Linux Setup:**
I repeated the exact setup on my Ubuntu endpoint for parity, creating a directory at /opt/company-data containing a file named test.txt.
Next I modified the Linux agent's ossec.conf file to monitor /opt/company-data in real-time and restarted the agent service.

**Testing:** Modifying and deleting test.txt successfully triggered the corresponding integrity and deletion events on my central dashboard

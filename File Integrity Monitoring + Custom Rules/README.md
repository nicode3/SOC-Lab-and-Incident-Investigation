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

## **Custom Guest Account Detection Rule**

Because the built-in Windows Guest account is disabled by default, attackers often try to enable it to create backdoor persistence. I engineered a custom Wazuh rule to detect when this specific account is activated.

### **Identifying the Telemetry:**

I manually enabled the Guest account on my Windows VM to generate raw log events.

I searched my Wazuh Archives and identified Event ID 4722 as the event code indicating an account was enabled.

I extracted the specific log fields to build a highly granular search query: *data.win.eventdata.targetUserName: Guest and data.win.system.eventID: 4722*

### **Creating Rule:**

I drafted an XML rule in my manager's local_rules.xml file using AI to help structure the basic logic

Later we confirm the rule being executed successfully after enabling the account.

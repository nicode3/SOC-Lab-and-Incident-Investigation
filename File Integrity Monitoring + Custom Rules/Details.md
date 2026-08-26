### **Windows Agent FIM Set Up**

Create a new directory in the Local C file called CompanyData and add a text file contain secret information

<img width="938" height="420" alt="image" src="https://github.com/user-attachments/assets/3a7b1541-e315-48d7-a72d-1ff05493c66e" />

We will then go into the ossec.conf file to configure the FIM 
In the file we mainly want to focus on the directory realtime line set to yes.
We can use this to check realtime file monitoring on the new CompanyData Directory we created
Add the Company Data file and save. Restart the Wazuh service.

<img width="893" height="621" alt="image" src="https://github.com/user-attachments/assets/27a14b88-a086-4ea5-9a00-2ffd1213e9d7" />
<img width="856" height="129" alt="image" src="https://github.com/user-attachments/assets/9b07ca73-bf75-4419-8ee2-fbcaac07c40f" />

Right Now we have no FIM events so we got back to the windows Vm to create some
We open payroll and add 123 to it after adding the configuration change. save it and check for events

<img width="1863" height="395" alt="image" src="https://github.com/user-attachments/assets/dd8f1b08-8da0-45a1-bfd3-586ff077eef8" />
<img width="935" height="677" alt="image" src="https://github.com/user-attachments/assets/c8b7f54a-ab09-4292-b7c7-44b580e68a2d" />
<img width="1908" height="198" alt="image" src="https://github.com/user-attachments/assets/2a1e8dc4-9230-4145-997e-863da6b5716d" />

Now I am going to create more telemetry for my FIM event logger by deleting the notes

<img width="789" height="604" alt="image" src="https://github.com/user-attachments/assets/92b823d6-45ef-44e8-adec-779c55d2f767" />
<img width="1088" height="224" alt="image" src="https://github.com/user-attachments/assets/31beb989-9dfe-49b1-947f-7b5fa73b570a" />

### **Ubuntu FIM Set Up**

We will set it up similar to windows so we make a company data directory that includes a test file for the Linux FIM

We open the ossec.conf file to configure the FIM to monitor our new directory
Restart service after making changes to config files

<img width="468" height="98" alt="image" src="https://github.com/user-attachments/assets/ccc5e83d-ed5c-4632-8e32-86e93f4d4289" />
<img width="536" height="171" alt="image" src="https://github.com/user-attachments/assets/2c4a4ff2-4e6d-4305-a627-7f00380485ff" />

We will next modify the test.txt file  and delete it afterwards to ensure that the FIM works and we will confirm that in the Wazuh Manager

<img width="1077" height="189" alt="image" src="https://github.com/user-attachments/assets/bb89bc98-64de-4f77-bbd8-46fc3ea88425" />

### **Custom Rule Set Up**

We create a query that will give us logs that show when specifically a guest account has been enabled
We used the EventID and Target User Name

<img width="509" height="55" alt="image" src="https://github.com/user-attachments/assets/40d44b67-90a1-4958-be9e-0e3bd9728a07" />
<img width="377" height="40" alt="image" src="https://github.com/user-attachments/assets/ae4ba7fe-3286-4bcb-a70f-2a4263ba1d11" />

We grab the data correlated to the Guest name and Event ID to create this query:

**data.win.eventdata.targetUserName: Guest AND data.win.system.eventID: 4722**

Now we will create a custom rule using AI along with our existing rules

*AI can be wrong, ensure that the rule contains the important facts like eventID and target user name*

There will be a reload option for the manager to restart in order for the rule to take effect

<img width="464" height="817" alt="image" src="https://github.com/user-attachments/assets/6ac9961c-d11d-4e17-ae12-6ab8c788de4b" />
<img width="1186" height="404" alt="image" src="https://github.com/user-attachments/assets/239de6c6-3388-4a22-a3da-5d088b8291a5" />
<img width="584" height="295" alt="image" src="https://github.com/user-attachments/assets/14c5c1a6-b921-4bba-85d8-e2a9a85125f1" />

We run the query in the discover tab and we find our rule being executed after the guest account was enabled

<img width="1902" height="92" alt="image" src="https://github.com/user-attachments/assets/830efeaa-7943-40ae-83bc-1c7ff1fe7ec4" />
<img width="974" height="81" alt="image" src="https://github.com/user-attachments/assets/94890429-af38-4991-99b9-82575d36fba6" />
<img width="1690" height="838" alt="image" src="https://github.com/user-attachments/assets/386491c7-3d25-408e-993e-ca037e1875b8" />


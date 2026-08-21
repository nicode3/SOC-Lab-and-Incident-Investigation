<div align="center">

# 🛡️ SOC Lab & Incident Investigation 🛡️
*A complete walkthrough of setting up enterprise monitoring tools.*

</div>

<div align="center">

## Windows 11 Wazuh Agent & Sysmon Setup

</div>

We ensure Sysmon is installed on our Windows 11 machine by checking for Sysmon logs in the Event Viewer after installation

<img width="650" height="500" alt="image" src="https://github.com/user-attachments/assets/9194f172-d651-436d-be5e-6c283914ff1f" />

We need to get rid of these lines in this file ossec.conf because their excluding security events with the IDs. We are only interest in seeing Sysmon data so we can remove this.

<img width="862" height="274" alt="image" src="https://github.com/user-attachments/assets/4e4b3059-ae14-41ff-89cb-764dbdeb4924" />

We need the change the Application name on the configuring line above the part we removed, so well get the name for Sysmon from the event viewer

<img width="505" height="166" alt="image" src="https://github.com/user-attachments/assets/f43a5b82-15a2-4271-bc1c-19554116dce9" />

Place the Sysmon name in between the location line

<img width="525" height="102" alt="image" src="https://github.com/user-attachments/assets/563de26d-027c-45a1-befb-5e7661da5a20" />

Then we always restart service after making changes to the ossec.conf file and then confirm Sysmon telemetry in Wazuh

<img width="843" height="319" alt="image" src="https://github.com/user-attachments/assets/54f5f173-df35-49b1-9ea7-137f8a37939a" />

we can now configure Wazuh and get the Windows 11 agent to call into Wazuh to generate malicious telemetry

1. select deploy agent

<img width="549" height="237" alt="image" src="https://github.com/user-attachments/assets/38d6978f-096b-43c5-a36e-cee83cc93e9c" />

2. Select windows and add the windows ip address

<img width="800" height="561" alt="image" src="https://github.com/user-attachments/assets/b7d99140-56a9-4526-8105-923b54db9254" />

3. change the agent name to your choice and copy the following commands and run it in the windows 11 powershell as administrator

<img width="800" height="519" alt="image" src="https://github.com/user-attachments/assets/6e18dcc9-2450-46c7-a095-c27e5e8a0a00" />

4. run the command and start the wazuh service with the command "net start wazuhsvc"

<img width="800" height="679" alt="image" src="https://github.com/user-attachments/assets/3d447ea4-507b-4976-9499-5508d4e7612c" />

Allow port 1515 and 1514 on the Wazuh manager to be able to allow the Wazuh agent to send events. Also if you are remote connected to the vm via ssh allow port 22 before turning on the firewall to avoid losing connection. 

<img width="345" height="110" alt="image" src="https://github.com/user-attachments/assets/dd12110b-68e9-42f7-9062-b25419e04cb7" />
<img width="353" height="57" alt="image" src="https://github.com/user-attachments/assets/6a2c4367-779c-4757-9956-70f10ca86a67" />

<img width="485" height="288" alt="image" src="https://github.com/user-attachments/assets/ee925191-c23e-411c-9c5f-6948942bee50" />

Now we check if the agent is active on the wazuh webpage. If you don't see it you need to go to services and restart Wazuh

<img width="453" height="433" alt="image" src="https://github.com/user-attachments/assets/e593f242-1755-4508-9455-a6b5a290ec93" />

I accidentally put in the wrong ip address when i first set up the first agent. You need to put the Wazuh Manager IP Address and not the VM's IP where your using the agent. This caused a permanent change in the ossec.conf file where it contains the official ip of the Wazuh Manager. 

If you did the same thing open the ossec.conf file through powershell to avoid permission error and change the ip address between the address line to the correct one. 

<img width="812" height="419" alt="image" src="https://github.com/user-attachments/assets/27859aa0-8238-4171-9bae-e0791319f701" />

<img width="479" height="321" alt="image" src="https://github.com/user-attachments/assets/538d366b-606b-4e79-baaa-f099fb570e76" />

after this wait 2 minutes and if it still doesn't show then restart services like shown earlier and it should have the agent running like so

<img width="800" height="645" alt="image" src="https://github.com/user-attachments/assets/7e5f2c1c-05b7-4c81-a09a-8a7f578cab06" />

Sysmon Telemetry Confirmed! Of course this ensures Sysmon data from the Windows 11 Vm is sending it to the Wazuh Manager Vm

<img width="650" height="500" alt="image" src="https://github.com/user-attachments/assets/4abb4dd3-94d4-4450-8a8c-e92dc54a4522" />

<div align="center">

## Ubuntu Wazuh Agent & Sysmon Setup

</div>

I installed an agent on it and made sure to change the IP in the oseec.conf file to the Wazuh Manager IP to connect it to the Wazuh Manager Service WebPage

<img width="307" height="89" alt="image" src="https://github.com/user-attachments/assets/7ff5ca89-a065-4d23-bf6e-ec88279973f7" />

<img width="1885" height="618" alt="image" src="https://github.com/user-attachments/assets/5fada2ee-cbad-4396-a023-c4561f2b24a9" />

Now we can check for data of our agent in the archives index
We can filter out VMs to narrow our search

<img width="260" height="69" alt="image" src="https://github.com/user-attachments/assets/aafc10de-c951-43a8-b331-867df58bed48" />

<img width="600" height="200" alt="image" src="https://github.com/user-attachments/assets/9718f5e5-f5d4-43e8-89e4-849714ceb142" />

<img width="530" height="396" alt="image" src="https://github.com/user-attachments/assets/d645e25a-787f-41af-b95f-1c09c78c9d97" />

This is where you can install sysmon for linux

<img width="609" height="161" alt="image" src="https://github.com/user-attachments/assets/59aeb3ae-f811-4dba-8752-4820d873d673" />
<img width="284" height="47" alt="image" src="https://github.com/user-attachments/assets/c591a743-9422-47f8-a5db-730d2068e690" />
<img width="1061" height="318" alt="image" src="https://github.com/user-attachments/assets/a6a3045a-a7df-4d38-ba86-6f857504f379" />
<img width="791" height="66" alt="image" src="https://github.com/user-attachments/assets/906b7923-44b9-468c-88d4-3c8a943555a5" />

Collect-all.xml confirms Sysmon installation

<img width="722" height="349" alt="image" src="https://github.com/user-attachments/assets/2f1daa25-9b53-41a7-a29a-06c0248fb34d" />

Sysmon Installed

<img width="877" height="321" alt="image" src="https://github.com/user-attachments/assets/b37694e1-250a-4813-a960-98bfadd8c027" />

Go into the directory to check for the Sysmon logs and we cant use tail to see the Linux Sysmon log and event ID of 11

<img width="324" height="28" alt="image" src="https://github.com/user-attachments/assets/9f15b4ae-7a59-4a95-8a6c-90d790aa38a2" />
<img width="809" height="277" alt="image" src="https://github.com/user-attachments/assets/19fc4486-7158-4d56-84be-f5dceb889510" />

Go back to Wazuh to check for Linux Sysmon Logs

<img width="679" height="387" alt="image" src="https://github.com/user-attachments/assets/785fd7da-0a73-4811-b80f-a4649a39ee4c" />

<img width="369" height="106" alt="image" src="https://github.com/user-attachments/assets/0b4dca2c-ba53-477f-942c-74ebdee0b4f5" />
<img width="1351" height="188" alt="image" src="https://github.com/user-attachments/assets/f4318b75-17fe-4e2f-92e0-1b162dc906a6" />











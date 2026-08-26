Running commands on Window's cmd to generate telemetry. By creating this telemetry we are looking for EventID 4726.

<img width="801" height="443" alt="image" src="https://github.com/user-attachments/assets/306a8328-0761-46b8-a7f3-8c182bf286c8" />

I was not getting the event ID I needed so I went back to the conf file and saw i needed to add a security channel. That is where the Wazuh Managers grab its logs for the Windows 11 VM security alerts

<img width="713" height="203" alt="image" src="https://github.com/user-attachments/assets/ae0b814c-829b-4456-ad7f-2e7c2122893b" />

<img width="1125" height="817" alt="image" src="https://github.com/user-attachments/assets/39da1d0f-7585-4fa4-9954-bc326214e416" />

By analyzing these logs we learn that each account associated with these alerts have SID that follows a specific architecture

|S| Indicates that the string is a SID |
|R |Indicates the revision level |
|X| Indicates the identifier authority value |
|Y| Represents a series of subauthority values, where _n_ is the number of values |

So in our example our SID is: S-1-5-21
At the end of the SID there are 4 numbers and that is a relative Identifier (RID) each account is given its own RID

## **Diving Deeper into Log Analysis:**

<img width="973" height="282" alt="image" src="https://github.com/user-attachments/assets/372ac8c2-a1e9-4595-beec-89940a26a389" />
<img width="217" height="19" alt="image" src="https://github.com/user-attachments/assets/027ad5a8-a1df-446f-964c-890d746d16ef" />

Looking at the screenshots above we see the subject is the source of where the log came from. The first screenshots can also tell us that the target account is the destination and the account name "nicol" performed these accounts under the computer name "NICOLAS-PC"

We learn UAC or User Account Codes are a sum of different codes. We can see more in detail by looking below at the screenshots below

<img width="325" height="69" alt="image" src="https://github.com/user-attachments/assets/36827fe3-80e3-47a7-8677-a137d1d02207" />
<img width="690" height="860" alt="image" src="https://github.com/user-attachments/assets/cda5b896-6a6d-437f-9853-4002b0be1556" />

In the smaller screenshot we see what the 0x15 is made up of. We can check what number these specifics from the UAC connect to.
We can also just start from the biggest number that would add up to 15 and in this scenario it would be 10. We would do the same for the following numbers.
So the next biggest number after 10 (making sure we are following the document above) would be 4 then 1.

## **No Account Name? No Problem.**

Sometimes logs won't show accounts name so we can utilize the SID + RID to fnd out exactly what account was affected. 
If we dig deeper we can find the what we were looking for next to "data.win.eventdata.targetUserName"
<img width="636" height="339" alt="image" src="https://github.com/user-attachments/assets/347f178b-d0ce-4865-a4f8-b347deb0fd95" />
<img width="688" height="67" alt="image" src="https://github.com/user-attachments/assets/0770427e-a935-4da2-9be4-b0d731f166f9" />

Anytime there is an accepted password under SSH logs you can infer that there was a successful authorization

<img width="626" height="300" alt="image" src="https://github.com/user-attachments/assets/44a9d967-7a33-41b9-92eb-d437a9250279" />
<img width="331" height="76" alt="image" src="https://github.com/user-attachments/assets/473d2314-7370-459a-8936-b5b4966086d5" />
<img width="845" height="40" alt="image" src="https://github.com/user-attachments/assets/dd32d93b-01f5-4b3e-96d7-e89a5bb02561" />

Whenever somebody successfully authenticates via ssh there is a session identifier that tracks that
The SSH session that was established from the windows vm into the ubuntu vm is being tracked under session id 20

<img width="180" height="59" alt="image" src="https://github.com/user-attachments/assets/36e44afe-c729-4042-8a85-812350700dee" />
<img width="948" height="63" alt="image" src="https://github.com/user-attachments/assets/7c771dd9-b52b-4c90-8d5d-b1a1da109be6" />
<img width="808" height="27" alt="image" src="https://github.com/user-attachments/assets/f30da4c8-d49f-4901-ae7e-207750aab7c5" />

A new session shown in the logs is when someone logs in and starts doing activity in the ssh session. You should be able to capture when they login and logoff as well

<img width="266" height="41" alt="image" src="https://github.com/user-attachments/assets/db875583-18a6-4ae8-a3ce-c38ae492131c" />
<img width="944" height="31" alt="image" src="https://github.com/user-attachments/assets/096665bc-26c9-4fd5-b684-c4b42463eca1" />
<img width="1218" height="50" alt="image" src="https://github.com/user-attachments/assets/0bf5b152-89b3-4730-8333-8afafc169b56" />

With the session now being closed we want to track if this is the same session that was opened earlier. We can see in the log that the session ID is not shown.
In this case we can look at the sshd number to match session activity. We go back to when the session was first opened to find the sshd we need and match it to the closed log sshd ID.

With details like this we can also use timestamps to find when sessions were opened and closed and can give us clues in our investigations









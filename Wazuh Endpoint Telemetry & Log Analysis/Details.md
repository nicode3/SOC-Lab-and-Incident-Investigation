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

Looking at the screenshots above we see the subject is the source of where the log came from.

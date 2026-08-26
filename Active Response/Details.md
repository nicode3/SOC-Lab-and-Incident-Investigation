Intentionally fail the login 3 times

We want to create a custom rule to specifically alert 3 failed ssh login attempts within 120 seconds

For now we are getting alerts that are not granular in its detection for what we are looking for

<img width="816" height="368" alt="image" src="https://github.com/user-attachments/assets/de49ae76-212d-4508-a62c-64c11f2b38a6" />
<img width="732" height="751" alt="image" src="https://github.com/user-attachments/assets/26bc0dc6-e2bd-421d-a12c-19673eb7436b" />

We use AI to help us create a custom rule to alert 3 failed login attempts within 120 seconds

<img width="574" height="119" alt="image" src="https://github.com/user-attachments/assets/2c4d7843-8d9e-46e8-abf0-ca5ea2ecb6fa" />
<img width="848" height="438" alt="image" src="https://github.com/user-attachments/assets/de50c25e-3bf3-4b3e-907c-515f35dbf639" />

We will fail login again 3 times to test the rule and we see it hit how it was originally meant to be

<img width="732" height="467" alt="image" src="https://github.com/user-attachments/assets/6736a2d1-0df8-465b-ae3d-39136b7c0c64" />
<img width="807" height="362" alt="image" src="https://github.com/user-attachments/assets/f67f017a-c160-45ff-a303-cc982aa509b5" />

Now we can configure active response so Wazuh can make automated actions

nano into /var/ossec/etc/ossec.conf and remove the content around this active response section

<img width="246" height="104" alt="image" src="https://github.com/user-attachments/assets/d733c325-d6a8-457f-8b00-ca1399c9e9ac" />

We will add this active response script to this section, run it on the local host where the alert came from (the agent responsible for generating the alert).

<img width="608" height="85" alt="image" src="https://github.com/user-attachments/assets/d8704efe-c7a1-4b24-992f-05f94b2105ea" />
<img width="297" height="145" alt="image" src="https://github.com/user-attachments/assets/42e9f687-3941-40bc-b0ce-06b1ef002f29" />

We check if our active response is implemented into the system, using "-L" allows us to see available active responses

<img width="762" height="525" alt="image" src="https://github.com/user-attachments/assets/6a784f7b-1831-480f-b623-05839a3b383c" />


I opened up two PowerShell windows the first one is an infinite ping to the ubuntu machine to see real time connectivity before doing three failed login attempts to test our new active response on the second opened window

We confirm this working by seeing the alert shown in our Wazuh

<img width="1050" height="638" alt="image" src="https://github.com/user-attachments/assets/a2ff3165-e3c7-4dac-83be-2dd67c2e3c08" />
<img width="773" height="293" alt="image" src="https://github.com/user-attachments/assets/e8829bb1-f76d-4978-b0b2-67f600b6a3ae" />

We can see that the IP trying to enter into the Ubuntu VM was marked with "DROP" 

<img width="437" height="210" alt="image" src="https://github.com/user-attachments/assets/311d770d-9606-4319-9613-48cfd46bf3ad" />

Now to unblock the IP we go into iptables in the Ubuntu machine and run the following commands

<img width="439" height="360" alt="image" src="https://github.com/user-attachments/assets/822b5202-9935-4555-b668-99e15e53e7ac" />
<img width="1063" height="489" alt="image" src="https://github.com/user-attachments/assets/222b312e-ded5-49e0-986c-7ee3b2bacb7a" />

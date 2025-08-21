---
layout: default
---

[Back to main page](https://ornaka.github.io)

This is my writeup about TryHackMe's PSEclipse-room, which is centered around Splunk

Link to the room: 

(https://tryhackme.com/room/posheclipse)



# Scenario

Scenario : You are a SOC Analyst for an MSSP (Managed Security Service Provider) company called TryNotHackMe .
A customer sent an email asking for an analyst to investigate the events that occurred on Keegan's machine on Monday, May 16th, 2022 . The client noted that the machine is operational, but some files have a weird file extension. The client is worried that there was a ransomware attempt on Keegan's device. 
Your manager has tasked you to check the events in Splunk to determine what occurred in Keegan's device. 
Happy Hunting!

## First Question:

A suspicious binary was downloaded to the endpoint. What was the name of the binary?

Since there are Sysmon logs available, I started by searching eventcode 3, which is used for network connections. Checking the DestinationIP-field, one stood out, which could be the adversaries IP address.

![Potential IP address of the adversary](/images/pic2.png)

After adding the IP address to the query, I almost immediately found the most likely name of the binary, but had  to be sure. Since I assumed the binary was downloaded via PowerShell, I queried only Powershell, checked the commandline field and found the encoded command for downloading the binary.  Decoding it in CyberChef confirmed that it was indeed the binary I saw earlier. 

![Encoded command](/images/pic3.png)

## Second question:

What is the address the binary was downloaded from? Add http:// to your answer & defang the URL.

The answer for this one was also present within the encoded command. I used CyberChef to decode it. 

## Third question:

What Windows executable was used to download the suspicious binary? Enter full path.

To get the full path of the executable, checking ParentCommandLine-field will help. 

Using the results gained in the first question, one can check ParentCommandLine to get the full path of the executable.

## Fourth question:

What command was executed to configure the suspicious binary to run with elevated privileges?

Saw this one while looking at CommandLine-field while verifying my answer to the first question. The /RU with schtasks.exe means it’ll run as SYSTEM-user when the task is executed. 

![Picture of the CommandLine-field](/images/pic4.png)

## Fifth question:

What permissions will the suspicious binary run as? What was the command to run the binary with elevated privileges?

The answer for this one was present within the picture in question four.

## Sixth question:

The suspicious binary connected to a remote server. What address did it connect to? Add http:// to your answer & defang the URL.

Because the binary is connecting to a remote server and question wants a specific address, I used a query containing name of the executable and TaskCategory="Dns query (rule: DnsQuery)". The answer was found within the QueryName-field in all of the five events the query resulted.

![One result of the query](/images/pic5.png)

## Seventh question:

A PowerShell script was downloaded to the same location as the suspicious binary. What was the name of the file?

Since I already knew the malicious binary was downloaded to /Temp and that the file I am looking for is a powershell script, I searched for C:\\Windows\\Temp\\*ps1. Notice that for the query to work the \'s need to be escaped with another \.

There were only two files directly in \Temp, other being the earlier malicious binary and second one being the script we’re looking for.

![Results](/images/pic6.png)

## Eight question:

The malicious script was flagged as malicious. What do you think was the actual name of the malicious script?

Result from earlier query showed also the file hashes of the script. Searching any of the hashes at VirusTotal revealed the true identity of the script.

![VirusTotal results](/images/pic7.png)

## Ninth question:

A ransomware note was saved to disk, which can serve as an IOC. What is the full path to which the ransom note was saved?

I used the actual name of the script found from VirusTotal as a query. Seven events were present, one of them being a .txt-file and correct answer.

![Field containing the answer](/images/pic8.png)

## Tenth question:

The script saved an image file to disk to replace the user's desktop wallpaper, which can also serve as an IOC. What is the full path of the image?

The answer for this one was also present within the seven events resulted from querying the actual name of the script.



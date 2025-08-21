---
layout: default
---

[Back to main page](https://ornaka.github.io)

This is my writeup about TryHackMe's PSEclipse-room, which is centered around Splunk

Link to the room 

https://tryhackme.com/room/posheclipse



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


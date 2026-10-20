# OhSINT Try Hack Me ROOM:

in this room i will try to understand and work on the fuandamental of OSINT first **what is OSINT ?**

**OSINT (open-source intellegence ) i**s the process of **gathering, analyzing, and disseminating information from publicly available sources** to produce actionable intelligence. Unlike classified methods, OSINT relies exclusively on open data such as **social media**, **public records**, **news media**, **commercial databases**, and **government publications**.

so if we know this much about osint lets start with the room

*   the first thing to do is to download the task file in this case i used the attack box because the file is already exist there in the following directory /Rooms/OhSINT , and i recommond using the attack box for the tools too if we got all of the we are good to go:
    
*   so before we start the questions lets start with accessing the meta data of the picture
    

![](https://cdn.hashnode.com/uploads/covers/6a77779d4416d3eea9accad0/91afd2dc-a3be-417c-ab74-959a298d8023.jpg align="center")

this is the given picture or shared by the user we are trying to find so to find the meta data we can use the attack box terminal

*   on the terminal we use this command to open the file on a tool called exif tools (is a tool used for to check the meta data of a picture)
    

```python
exiftool /root/Rooms/OhSINT/WindowsXP.jpg
```

![](https://cdn.hashnode.com/uploads/covers/6a77779d4416d3eea9accad0/f716fc14-a9dd-4150-bf20-40922c0e470e.png align="center")

the out put for the command is this and this is also the meta data of the pic so we can continue :

the first question is : ***What is this user's avatar of?***

*   the first step is to find who is the owner of this file and we can see a name under the copyright so lets get than name and start the search, we can easly start by searching the name\*\*(OWoodflint)\*\* on google
    

![](https://cdn.hashnode.com/uploads/covers/6a77779d4416d3eea9accad0/c032ccde-f3c7-4ca5-b0c7-c34616e67f34.png align="center")

good we got 2 links connected to the name so lets explore them one by one

![](https://cdn.hashnode.com/uploads/covers/6a77779d4416d3eea9accad0/9a970693-c937-4c7e-a321-4602a3841b66.png align="center")

on the first link i find his x account and there is some infos but for now we can see that the avatar is a cat lets see the second one

![](https://cdn.hashnode.com/uploads/covers/6a77779d4416d3eea9accad0/23cad4f2-3428-4525-8bf0-a343f7a6202f.png align="center")

this one is his github account and the avatar is random so the answer for the first one will be ***"cat"***

\*\**2nd question:What city is this person in?*

*   this one is an easy one we can see that on the persons github he wrote that he is from ***"london"*** so that will be the answer
    

***3rd question:What is the SSID of the WAP he connected to?***

*   while we were on his x account we can see that he tweeted his BSSID (B4:5D:50:AA:86:41) in this case i used a website called [`wigle.net`](http://wigle.net)
    
*   wigle (Wireless Geographic Logging Engine) is a powerfull database that maps Wi-Fi networks worldwide. It collects and organizes BSSIDs, SSIDs, and their approximate locations from various sources. By searching for the BSSID on Wigle, we can uncover the SSID and get closer to out answer.
    

Once you’re set, enter the BSSID you found into the **BSSID** search parameter and click on **Filter**. After the query updates, zoom out completely to reveal the world map.Since we already know he is in London, navigate over to that area. You should see a **red ring** marking the location of the access point.Press enter or click to view image in full size

Press enter or click to view image in full size

![](https://cdn.hashnode.com/uploads/covers/6a77779d4416d3eea9accad0/d653aca5-1bbe-4beb-8d94-0b5b65f7d23c.png align="center")

Simply follow the ring, zooming in step by step, until you reach street level, where the SSID ***"(UnileverWiFi)"*** will be visible uncovering the answer of the third question.

![](https://cdn.hashnode.com/uploads/covers/6a77779d4416d3eea9accad0/18b3a8f7-b1d1-4063-b8a0-acd7e579f7fa.png align="center")

***4th question :What is his personal email address?***

***this is also an easy one we can see that he put his email on github so its easy***

***"OWoodflint@gmail.com"***

***5th question : What site did you find his email address on?***

i find his email on github page the answer is **"GitHub"**

**6th question: Where has he gone on holiday?**

in this we have to scan every thing we can find for that lets use the wordpress link he left on the github to see if we can find any thing

![](https://cdn.hashnode.com/uploads/covers/6a77779d4416d3eea9accad0/c35bdeab-63fe-4ba7-98c9-d9362bd7453b.png align="center")

so he is in ***"new york"*** for holiday

***7th question:What is the person's password?***

*   in this case i can not find it by just scrolling around we have to dig dipper like i try every way to find the password by just scrolling through his social media account but there is nothing the only thing left is looking inside his wordpress source code so lets do that
    

![](https://cdn.hashnode.com/uploads/covers/6a77779d4416d3eea9accad0/e54e24a4-3441-43e6-bfff-306555d81277.png align="center")

and after a lot of time scrolling through the source code i did find something that look suspicies and its not visible on the website it self its only on the source code so lets check that and yes it is the password ***"pennYDr0pper.!"***

***conclusion***

*   on this page we can learn that we can gather info with out doing to much , and easily by just exploring what is left by the user him self so osin is one essencial thing for cybersecurity .
    

***Thank you for reading!***
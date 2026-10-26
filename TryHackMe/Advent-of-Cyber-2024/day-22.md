# Advent of Cyber 2024 - Day 22: It's because I'm kubed, isn't it?

{% embed url="https://tryhackme.com/room/adventofcyber2024" %}

## Overview & Investigation Walkthrough

1. What is the name of the webshell that was used by Mayor Malware?

   We can view the remote logs from the apache server to answer the following three questions.

   ```cmd
   cd /var/ubuntu/dfir_artefacts

   nano pod_apache2_access.log 
   ```

   At the end of the logs we can see some interesting entries.

   ![Webshell](https://raw.githubusercontent.com/Kevinovitz/TryHackMe_Writeups/main/adventofcyber2024/Advent_of_Cyber_2024_Day_22_Webshell.png)

   ><details><summary>Click for answer</summary>shelly.php</details>

2. What file did Mayor Malware read from the pod?

   ><details><summary>Click for answer</summary>db.php</details>

3. What tool did Mayor Malware search for that could be used to create a remote connection from the pod?

   ><details><summary>Click for answer</summary>nc</details>

4. What IP connected to the docker registry that was unexpected?

   For the next three questions we need to look at the docker log file `docker-registry-logs.log`.

   ```cmd
   cat docker-registry-logs.log | grep "HEAD" | cut -d ' ' -f 1
   ```

   ![Ips](https://raw.githubusercontent.com/Kevinovitz/TryHackMe_Writeups/main/adventofcyber2024/Advent_of_Cyber_2024_Day_22_Ips.png)

   ><details><summary>Click for answer</summary>10.10.130.253</details>

5. At what time is the first connection made from this IP to the docker registry?

   Lets look at all requests made by this ip to find the first request.

   ```cmd
   cat docker-registry-logs.log | grep "10.10.130.253"
   ```

   ![Requests](https://raw.githubusercontent.com/Kevinovitz/TryHackMe_Writeups/main/adventofcyber2024/Advent_of_Cyber_2024_Day_22_Requests.png)

   ><details><summary>Click for answer</summary>29/Oct/2024:10:06:33 +0000</details>

6. At what time is the updated malicious image pushed to the registry?

   To find the update image time, we should filter on the `patch` request.

   ```cmd
   cat docker-registry-logs.log | grep "10.10.130.253" | grep "PATCH"
   ```

   ![Patch](https://raw.githubusercontent.com/Kevinovitz/TryHackMe_Writeups/main/adventofcyber2024/Advent_of_Cyber_2024_Day_22_Patch.png)

   ><details><summary>Click for answer</summary>29/Oct/2024:12:34:28 +0000</details>

7. What is the value stored in the "pull-creds" secret?

   To get the secret that was pulled we can use the following command.

   ```cmd
   kubectl get secret pull-creds -n wareville -o jsonpath='{.data.\.dockerconfigjson}' | base64 --decode
   ```

   ![Secret](https://raw.githubusercontent.com/Kevinovitz/TryHackMe_Writeups/main/adventofcyber2024/Advent_of_Cyber_2024_Day_22_Secret.png)

   ><details><summary>Click for answer</summary>{"auths":{"http://docker-registry.nicetown.loc:5000":{"username":"mr.nice","password":"Mr.N4ughty","auth":"bXIubmljZTpNci5ONHVnaHR5"}}</details>

8. Enjoy today's lesson? Check out ourIntro to Kubernetesfor a more in-depth introduction to Kubernetes!

## 🏁 Questions & Answers

| Question | Answer |
|---|---|
| What is the name of the webshell that was used by Mayor Malware? | `shelly.php` |
| What file did Mayor Malware read from the pod? | `db.php` |
| What tool did Mayor Malware search for that could be used to create a remote connection from the pod? | `nc` |
| What IP connected to the docker registry that was unexpected? | `10.10.130.253` |
| At what time is the first connection made from this IP to the docker registry? | `29/Oct/2024:10:06:33 +0000` |
| At what time is the updated malicious image pushed to the registry? | `29/Oct/2024:12:34:28 +0000` |
| What is the value stored in the "pull-creds" secret? | `{"auths":{"http://docker-registry.nicetown.loc:5000":{"username":"mr.nice","password":"Mr.N4ughty","auth":"bXIubmljZTpNci5ONHVnaHR5"}}` |

> **Note:** Flags are redacted as `flag{hidden}` in accordance with responsible disclosure guidelines. The step-by-step commands above provide the verified reproduction path.

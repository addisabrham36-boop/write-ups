# Internal: TryHackMe Walkthrough

Room: https://tryhackme.com/room/internal

Internal is a black-box penetration-testing challenge. The brief allows an external, web application, and internal assessment of the assigned virtual machine. The required proof is the user and root flags. Only the IP assigned by TryHackMe is in scope, and the hostname internal.thm must point to that address.

## 1. Configure the hostname and scan

Start the machine and record its current IP. Add this mapping to /etc/hosts on the AttackBox or testing machine:

    MACHINE_IP internal.thm

Replace MACHINE_IP with the address assigned by the room. Then scan the target:

    nmap -sC -sV -oN internal-nmap.txt MACHINE_IP

The intended environment exposes SSH and HTTP. Confirm the actual output from your own instance. Open the web service and inspect the default page, then enumerate paths:

    gobuster dir -u http://internal.thm/ -w /path/to/wordlist.txt

Use a wordlist available in the AttackBox. The target's web content leads to a WordPress installation. Keep the hostname mapping in place because the application may expect internal.thm rather than the raw IP.

## 2. Enumerate WordPress

Inspect the WordPress pages and use WPScan in the authorised lab to identify the version, visible users, plugins, themes, and other public information. For example:

    wpscan --url http://internal.thm/wordpress/ --enumerate u

Adjust the URL to match the actual WordPress path found on the target. The enumeration output may reveal usernames that can be used for the next authentication test. Do not treat a discovered username as proof that the account is compromised.

## 3. Test the WordPress login

The intended path uses a weak WordPress administrator password. Use the username and endpoint identified during enumeration. If the room calls for a password test, use WPScan's password-attack function or another controlled method with a lab wordlist and a conservative request rate.

Do not copy a password from another walkthrough or test unrelated services. Confirm any candidate by logging in through the intended WordPress interface. The lesson is that weak passwords and insufficient rate limiting can turn a public login page into an initial-access path.

## 4. Obtain a web shell through the theme editor

Once the WordPress administrator account is available, inspect the theme-editing functionality. In the vulnerable lab, the ability to modify a PHP theme file provides a path to server-side execution. The usual proof is to modify a file that is executed by the web server and then trigger it through the browser.

Use only the provided lab and a callback address reachable from the AttackBox. Start a listener if the room's instructions call for a reverse shell, then trigger the modified page. Verify the resulting shell identity with whoami and id. A web shell typically runs with the web-server account, not root.

Do not make this change to a real WordPress site. In production, theme editing should be disabled for ordinary administrators and file integrity should be protected.

## 5. Find the user flag and enumerate the host

After obtaining the initial shell, inspect the filesystem and application configuration. Record the current user, working directory, and host. Look for the user flag in the path indicated by the room and submit it in TryHackMe. The flag value is not included in this public note.

Continue enumeration by checking readable configuration files, running processes, network listeners, and application directories. Keep the scope limited to the assigned machine. Do not assume that database credentials found in a configuration file automatically work as Linux credentials; test each credential only against the service it belongs to and within the lab.

The intended chain includes a note revealing that Jenkins is available internally. This is a clue to investigate the host's network and internal services rather than scanning unrelated machines.

## 6. Discover the internal Jenkins service

From the shell, inspect network interfaces, routes, and listening or connected services. A service that is bound to an internal interface or container network may not be reachable directly from the AttackBox. The note in the lab provides the clue to the Jenkins service and its internal address.

Use SSH port forwarding to expose only the intended internal service through your authorised shell. The general pattern is:

    ssh -L 8080:INTERNAL_JENKINS_IP:8080 USERNAME@TARGET_IP

Replace the placeholders with the values discovered in the room. Keep the tunnel open, then access the forwarded service at http://127.0.0.1:8080 from the machine where the tunnel runs. Do not assume the internal address or port from another instance.

## 7. Authenticate to Jenkins and obtain the next shell

The room's intended path requires discovering weak Jenkins credentials. Inspect the login page and use the evidence from the lab to identify the correct account. If the exercise requires password testing, keep it limited to the Jenkins service and use the supplied wordlist or hints. Verify the credential by logging in through the forwarded local endpoint.

Jenkins can execute build steps and scripts, so administrative or job-configuration permissions may lead to code execution in the environment where Jenkins runs. In this lab, use the task's intended mechanism to obtain the next shell and verify the account and host. Do not assume a Jenkins shell is automatically root on the host; it may run inside a container.

## 8. Escalate from the internal environment

Once the second shell is established, inspect the filesystem, running services, and configuration for the final privilege boundary. The room's intended chain includes credentials stored in an accessible file. Follow the actual file path and evidence from the active machine rather than inventing a credential.

Verify root access with whoami and id before reading the root flag. The flag is omitted from the public write-up; retrieve it from the live instance and submit it in TryHackMe.

## 9. Reporting and remediation

The findings in this chain demonstrate several separate controls that failed:
- Weak WordPress administrator credentials and inadequate login protection.
- Excessive ability to edit executable theme files from the web interface.
- Sensitive credentials or internal service information stored in accessible files.
- An internal Jenkins service protected by weak authentication or unsafe permissions.
- A container or service boundary that does not sufficiently restrict the impact of Jenkins execution.

Fix each weakness independently. Enforce MFA and rate limits, disable production theme editing, restrict file permissions, keep secrets out of web-accessible files, protect Jenkins with strong authentication, limit job permissions, and isolate containers and internal services.

## What I took from this room

Internal is a chain rather than a single exploit: web enumeration leads to WordPress, WordPress leads to a web shell, host enumeration reveals an internal Jenkins service, and Jenkins access leads to the final privilege boundary. Each step should be supported by evidence from the live machine, and both flags should be redacted as flag{hidden} in a public write-up.
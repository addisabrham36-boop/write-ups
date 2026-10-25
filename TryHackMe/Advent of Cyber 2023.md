# Advent of Cyber 2023 — detailed day-by-day walkthrough

Room: https://tryhackme.com/room/adventofcyber2023

This is the **24-day Advent of Cyber 2023 event**, with a separate final courtroom task and completion pages. It is not the 2025 Prep Track, which contains ten warm-up challenges. I am keeping each day separate because the tools and evidence change throughout the event.

The actual target IP, task files, and lab output must come from the running TryHackMe room. I use `TARGET_IP` and similar names below as placeholders, never as real addresses. Actual flags are redacted as `flag{hidden}` for this public copy. Where a result depends on the lab's own logs, files, or chatbot responses, I need to read that result instead of inventing it.

## Set up each day's lab

1. Read the day's objectives and note whether it requires a lab machine, AttackBox, website, downloadable file, or more than one of these.
2. Start only the machines specified in that day's task and wait for the services to finish booting.
3. Use the provided split view or the direct lab URL shown by TryHackMe.
4. Keep the day's target details separate from the previous day's. A target IP can change between tasks.
5. Record the command, its important output, and why that output answers the question. The final answer should be traceable to evidence.

---

## Day 1 — Machine Learning: Chatbot, tell me, if you're really safe?

### Goal
Investigate prompt injection against Van Chatty, an internal chatbot, and discover information that should not be exposed to an ordinary user.

### Steps
1. Start the Day 1 lab machine and wait the instructed few minutes for the web service to start.
2. Open the lab URL shown on the connection card. Do not scan the TryHackMe host itself; only use the assigned lab application.
3. Begin with a normal question to understand the chatbot's intended role and the kind of information it answers.
4. Read the task's explanation of prompt injection. The vulnerability occurs when the model treats untrusted user instructions as higher-priority directions or reveals information that the application should keep private.
5. Ask questions that follow the room's hints and inspect the answers for the requested information. Change one part of the prompt at a time so I can tell which instruction influenced the output.
6. If the bot refuses, compare the refusal with the task's examples and continue using the room's intended clues. Do not assume every refusal means the target is secure.
7. Record the personal email address requested by the task and the flag shown in the supplied environment.

### Why this matters
A prompt is not a reliable access-control boundary. Sensitive information should not be placed in model context unless the user is authorised to see it, and retrieved data must be checked against server-side permissions.

**Flag:** `flag{hidden}`

---

## Day 2 — Log Analysis: O Data, All Ye Faithful

### Goal
Investigate the logs supplied by the task to reconstruct suspicious activity and identify the evidence requested in the questions.

### Steps
1. Start the Day 2 lab and inspect the working directory:

   ```bash
   pwd
   ls -lah
   ```

2. Identify the log file named in the task and inspect its beginning and end:

   ```bash
   head -n 40 LOGFILE
   tail -n 40 LOGFILE
   ```

3. Search for important words, usernames, paths, timestamps, or event types that appear in the task's scenario:

   ```bash
   grep -inE 'login|failed|admin|password|download|flag' LOGFILE
   ```

4. Replace `LOGFILE` with the actual file name. If the search produces too many results, narrow the search by the relevant date or field.
5. For each suspicious event, record the timestamp, source address, action, and outcome. Use surrounding lines to confirm that the events belong to the same activity.
6. Answer the room's questions from the log evidence, including any account or event detail requested. Do not assume that the earliest event is the first malicious action.

### Why this matters
Logs provide a timeline of what a system observed. The best analysis correlates multiple events and keeps the source data intact, rather than treating one suspicious line as the entire incident.

**Flag:** `flag{hidden}`

---

## Day 3 — Brute Forcing: Hydra is Coming to Town

### Goal
Understand password complexity and use Hydra against the authorised login service in the lab.

### Steps
1. Start the assigned lab machine and note its IP and web port from the task.
2. Read the login page and determine what field is being tested. This task uses a short numeric PIN, so the candidate set is much smaller than a normal long password.
3. Work out the size of the search space. For a PIN with (n) decimal digits, there are (10^n) possible values if leading zeroes are allowed.
4. If the task asks for a wordlist, generate the exact candidate range using the tool described in the room. For example, a three-digit list can be generated with Crunch using the required character set and length.
5. Inspect the login form's request in the browser developer tools or follow the endpoint and form parameters documented by the task.
6. Use the Hydra command pattern given by the room, replacing the host, port, endpoint, form field, and failure text with the actual values in the lab. Do not paste a generic form string without matching it to the real request.
7. Watch Hydra's result and distinguish a successful login from a redirect or error that can look similar. Use the recovered PIN at the intended door-control page and retrieve the backup tapes as instructed.

### Why this matters
Brute force succeeds when a password has a small search space and the service permits repeated guesses. Rate limits, lockouts, MFA, and longer random credentials make this attack much harder.

**Flag:** `flag{hidden}`

---

## Day 4 — Brute Forcing: Baby, it's CeWLd outside

### Goal
Create a context-specific wordlist and use it against the authorised service in the room.

### Steps
1. Start the lab and read the target story. Identify the company site or other public content the room explicitly provides for wordlist generation.
2. Use the provided site as the source for CeWL. A typical command structure is:

   ```bash
   cewl -w custom-wordlist.txt -d 2 -m 5 http://TARGET_SITE
   ```

3. Replace `TARGET_SITE` with the URL supplied by the room. The depth and minimum word length are examples; use the parameters the task specifies.
4. Inspect the generated list:

   ```bash
   head -n 30 custom-wordlist.txt
   wc -l custom-wordlist.txt
   ```

5. Check that the list contains words from the target's own context rather than unrelated dictionary entries.
6. Follow the room's authentication-testing instructions and use Hydra only against the assigned login service. Confirm the username, form fields, and failure response from the actual request.
7. When a candidate credential is found, verify it by using the intended lab login and record the result requested by the room.

### Why this matters
A targeted wordlist can outperform a generic one when an organisation uses names, product terms, or other predictable words in passwords. The defence is to use unique random passwords and prevent repeated guessing.

**Flag:** `flag{hidden}`

---

## Day 5 — Reverse Engineering: A Christmas DOScovery, Tapes of Yule-tide Past

### Goal
Use the legacy DOS environment and the provided recovery tool to inspect the backup-tape artefacts.

### Steps
1. Start the machine and connect using the credentials and method shown by the task. Wait for the desktop to finish loading.
2. Read the task's instructions before running commands. This is a recovery investigation, not a request to brute-force the machine.
3. Inspect the provided files and identify the older tool that runs in DOS. Record its name and the input files it expects.
4. Use the supplied DOS environment to open the tool. DOS commands are different from Linux commands, so check the on-screen help or the task's command examples.
5. Follow the recovery workflow to inspect the backup tape and locate deleted or recoverable data. Keep the original artefacts unchanged.
6. When the tool reports a recovered record, read the surrounding context to understand what the record represents. Submit the requested evidence and flag from the recovered data.
7. If a command fails, check the current drive and directory, and verify the exact filename. Do not assume a modern shell command exists in DOS.

### Why this matters
Digital forensics often involves legacy file formats and tools. The key is to preserve the original evidence and make a reproducible record of the recovery steps.

**Flag:** `flag{hidden}`

---

## Day 6 — Memory Corruption: Memories of Christmas Past

### Goal
Understand how an unsafe program can allow input to overwrite adjacent memory and alter the program's behaviour.

### Steps
1. Start the lab machine and open the game page provided by the task.
2. Read the explanation of buffers, memory layout, and why an unchecked input length can cause corruption.
3. Interact with the game normally first and record the intended behaviour. This gives a baseline for comparison.
4. Follow the challenge's instructions to determine the expected input size and the program's input handling. If the task provides source code or a binary, inspect the relevant function rather than guessing.
5. Use the controlled input supplied by the room to observe when the behaviour changes. Change the input length systematically and record the response.
6. Follow the room's intended buffer-overflow exercise to modify the relevant in-memory value and reach the required game state.
7. Submit the flag produced by the lab and note what changed in the program's behaviour.

### Why this matters
A buffer overflow occurs when code writes more data than the allocated buffer can hold. Safe languages, bounds checks, compiler protections, and careful input validation reduce this risk. Exploit testing belongs only in the supplied lab.

**Flag:** `flag{hidden}`

---

## Day 7 — Log Analysis: 'Tis the season for log chopping!

### Goal
Use log-analysis techniques to find important activity in the supplied data.

### Steps
1. Start the lab and list the available log files.
2. Identify the relevant file by reading the task scenario and checking filenames, timestamps, and sizes.
3. Use basic text-processing commands to narrow the dataset:

   ```bash
   grep -in 'keyword' LOGFILE
   cut -d ' ' -f 1 LOGFILE | sort | uniq -c | sort -nr
   ```

4. Replace the placeholder filename and delimiter with the actual format. The example shows how to count repeated values in the first space-separated field; it may need adjusting if the log uses a different format.
5. Look for spikes in repeated requests, repeated usernames, suspicious paths, or unusual event IDs.
6. For any candidate event, inspect the full line and its neighbours to confirm the timestamp and outcome.
7. Use the resulting evidence to answer the task questions. Do not treat a high request count alone as proof of malicious activity; correlate it with the event details.

### Why this matters
Simple command-line tools can answer many log questions quickly, but parsing depends on the actual format. A reliable analyst checks the fields before applying a pipeline.

**Flag:** `flag{hidden}`

---

## Day 8 — Disk Forensics: Have a Holly, Jolly Byte!

### Goal
Use FTK Imager and the provided disk image to recover artefacts and understand what happened on the workstation.

### Steps
1. Start the lab machine and connect using the provided desktop access.
2. Open FTK Imager from the desktop. The task includes the relevant artefacts, so there is no need to download unrelated tools.
3. Open the evidence image through the program's evidence tree and wait for the directory listing to populate.
4. Browse the file system and pay attention to deleted entries, user directories, documents, and timestamps mentioned by the scenario.
5. Use the task's clues to locate the relevant deleted or hidden artefact. If the interface shows a deleted file, inspect it without modifying the evidence image.
6. Export a file only if the task requires it, and save it to the analysis workspace rather than the evidence source.
7. Read the recovered content and answer the questions about the incident and the evidence trail.

### Why this matters
Disk forensics recovers data from file systems and storage images. Maintaining evidence integrity is important: opening an image read-only and documenting exports helps keep the analysis reproducible.

**Flag:** `flag{hidden}`

---

## Day 9 — Malware Analysis: She sells C# shells by the C2shore

### Goal
Perform static analysis of the supplied .NET malware sample to identify its behaviour and command-and-control information.

### Steps
1. Start the lab machine and use the supplied analyst login if the task provides one.
2. Locate the defanged sample in the artefacts folder. The room explicitly focuses on static analysis, so do not execute the sample.
3. Identify the file type and calculate a hash:

   ```bash
   file SAMPLE
   sha256sum SAMPLE
   strings -n 6 SAMPLE | head -n 100
   ```

4. Open the sample in the .NET decompiler or analysis tool provided by the lab. .NET assemblies often contain intermediate language and metadata that can be decompiled into readable source-like code.
5. Inspect the entry point and search for networking functions, hard-coded URLs, IP addresses, request paths, encryption keys, and configuration values.
6. Trace the relevant function to see how the sample constructs its C2 communication. Record the domain or path requested by the task.
7. Submit the answers based on the code and artefacts. Do not run the malware to “confirm” a URL when the task is designed for static analysis.

### Why this matters
Static analysis reveals what a program is designed to do without running it. Hashes identify the sample, while decompilation helps connect embedded strings to the code paths that use them.

**Flag:** `flag{hidden}`

---

## Day 10 — SQL Injection: Inject the Halls with EXEC Queries

### Goal
Investigate the vulnerable web application and understand how SQL injection changes a database query.

### Steps
1. Start the lab machine and wait for the web service to initialise.
2. Open the application URL supplied by the task.
3. Inspect the page and identify the input that is used to retrieve records. Begin with an ordinary valid input to understand the normal response.
4. Follow the room's SQL injection lesson and test the provided input in the authorised application. Change one part of the input at a time and compare the result.
5. If the page reveals an error or different number of records, use that as evidence to reason about the underlying query. Do not assume every error is SQL injection.
6. Follow the task's steps to enumerate the information required by its questions, keeping each query within the assigned machine.
7. Submit the requested values and record the vulnerable code pattern described by the room.

### Why this matters
SQL injection occurs when untrusted input changes SQL syntax. Parameterised queries keep data separate from code, while least-privilege database accounts reduce the impact of a successful exploit.

**Flags:** `flag{hidden}` for each actual flag in the room.

---

## Day 11 — Active Directory: Jingle Bells, Shadow Spells

### Goal
Investigate the Active Directory environment and understand the authentication and privilege relationships described in the task.

### Steps
1. Start the lab environment and read the connection card carefully. Some Active Directory tasks require the supplied credentials and the lab's own network.
2. Use the exact host and domain information given by the task. Do not scan the TryHackMe platform or any other network.
3. Enumerate the domain information that the room asks for, using the tools and commands shown in the task.
4. Read the task's explanation of the relevant authentication mechanism and the “shadow” technique. Identify which object, credential, or permission is being changed or abused.
5. Use the supplied account to follow the lab's intended attack path. Record the output that proves the misconfiguration, such as an exposed account relationship or an unexpected authentication result.
6. Continue only along the steps described by the room. Confirm each privilege change with the relevant command or application response before proceeding.
7. Submit the requested account, privilege, or flag answers.

### Why this matters
Active Directory security depends on the relationships between identities, credentials, groups, and services. A single overly permissive account or authentication configuration can enable further access. The defensive fix should address the underlying permission or credential issue, not only the final command.

**Flag:** `flag{hidden}`

---

## Day 12 — Defence in Depth: Sleighing Threats, One Layer at a Time

### Goal
Apply multiple defensive controls and understand how the Diamond Model helps describe an intrusion.

### Steps
1. Read the task's description of the Diamond Model. Its four core features are adversary, capability, infrastructure, and victim.
2. Use the supplied incident details to place each clue into the correct part of the model. For example, a suspicious tool is a capability, while a command-and-control host is infrastructure.
3. Identify the defensive control the task asks me to configure, such as a firewall rule or honeypot.
4. On the supplied lab machine, inspect the current configuration before changing it. Record the existing rules or listening services.
5. Follow the room's steps to create the specific firewall rule. Confirm the rule is present and test the intended traffic path using the task's own test.
6. Configure the honeypot as the task instructs, then verify that it is listening and that the expected connection or alert appears.
7. Answer the questions based on the observed behaviour and the model mapping.

### Why this matters
Defence in depth means multiple independent controls support one another. A firewall can restrict access, monitoring can detect suspicious activity, and a honeypot can provide a signal when someone probes a decoy service.

**Flag:** `flag{hidden}`

---

## Day 13 — Intrusion Detection: To the Pots, Through the Walls

### Goal
Use intrusion-detection concepts and honeypot telemetry to identify suspicious network activity.

### Steps
1. Start the lab machine and open the relevant monitoring interface or terminal.
2. Read the task's explanation of intrusion detection and the difference between a detection signal and a confirmed incident.
3. Inspect the supplied alerts, traffic records, or honeypot logs. Identify the source, destination, timestamp, and activity type for each relevant entry.
4. If the task includes a network scan or connection exercise, run only the command specified against the assigned lab host.
5. Compare the observed connection with the expected normal behaviour described by the room. A connection to a decoy port may be suspicious, but I should use the surrounding evidence before classifying it.
6. Map the activity to the detection rule or defensive control the task asks about and submit the resulting answers.

### Why this matters
Intrusion detection gives defenders visibility into events that prevention controls may not block. Good alert triage separates normal activity, suspicious activity, and confirmed compromise.

**Flag:** `flag{hidden}`

---

## Day 14 — Machine Learning: The Little Machine That Wanted to Learn

### Goal
Understand the basics of machine learning and use the provided dataset or notebook to predict defective toys.

### Steps
1. Start the lab machine and open the notebook or application linked from the task.
2. Read the data description before running the model. Identify which fields are inputs and which field is the label the model is expected to predict.
3. Follow the notebook cells in order. Inspect the dataset shape, column names, and example rows before training.
4. Separate the features from the label as the task describes. Avoid including the answer column among the inputs, because that would leak the expected answer into the model.
5. Run the training and evaluation cells. Record the metric that the task asks for and understand what it measures.
6. Use the trained model to predict the required examples and compare the results with the task's expected behaviour.
7. Submit the prediction or flag shown by the room.

### Why this matters
Machine learning finds patterns in data, but the result depends on the quality of the data and the model setup. A model can perform well on training data and still fail on unseen examples.

**Flag:** `flag{hidden}`

---

## Day 15 — Machine Learning: Jingle Bell SPAM: Machine Learning Saves the Day!

### Goal
Use a machine-learning spam detector to classify messages and understand the relationship between features and predictions.

### Steps
1. Start the lab machine and open the notebook or application provided by the task.
2. Read the dataset columns and determine which field contains the message text and which field contains the spam/ham label.
3. Follow the notebook's preprocessing steps. Text classifiers often need the text normalised and converted into numeric features before a model can learn from it.
4. Run the training cells and inspect the evaluation output. If the task asks about precision, recall, or accuracy, read the metric definition rather than treating them as interchangeable.
5. Test the sample messages provided by the room. Compare the model's predictions with the intended labels.
6. If a task asks me to improve a model or identify a result, change only the setting the room directs and rerun the relevant cell.
7. Submit the answer from the model output.

### Why this matters
A spam classifier can make false positives and false negatives. The appropriate metric depends on the cost of each error, and suspicious messages should still be reviewed rather than judged by one score alone.

**Flag:** `flag{hidden}`

---

## Day 16 — Machine Learning: Can't CAPTCHA this Machine!

### Goal
Understand how a machine-learning model can help solve a visual CAPTCHA and how that changes the assumptions behind automated login protection.

### Steps
1. Start the lab machine and open the application supplied by the task.
2. Read the CAPTCHA instructions and inspect the image or challenge data.
3. Follow the provided notebook or model workflow to preprocess the image. The input format and image size must match what the model expects.
4. Run the inference or prediction step and inspect the output. If the task asks about a class label, map the output to the label list provided by the notebook.
5. Follow the task's controlled demonstration to submit the predicted answer to the lab login form.
6. If the first prediction fails, inspect the preprocessing and label mapping before assuming the model itself is broken.
7. Submit the requested answer and record the limitation demonstrated by the exercise.

### Why this matters
A CAPTCHA is not a replacement for authentication controls. Rate limits, MFA, abuse detection, and secure session management are needed because automated solvers can defeat some visual challenges.

**Flag:** `flag{hidden}`

---

## Day 17 — Traffic Analysis: I Tawt I Taw a C2 Tat!

### Goal
Analyse network traffic to identify suspicious communication with command-and-control infrastructure.

### Steps
1. Start the lab machine and locate the PCAP or traffic evidence provided by the task.
2. Open the capture in Wireshark and begin with **Statistics → Conversations** and **Protocol Hierarchy** to see which hosts and protocols are most active.
3. Apply the relevant display filters, such as `dns`, `http`, or `ip.addr == TARGET_IP`, based on the task's clues.
4. Inspect suspicious DNS queries, HTTP requests, or repeated outbound connections. Record the destination, timing, and request pattern.
5. Use **Follow TCP Stream** when a connection contains readable application data. Keep the client request and server response together so the context is not lost.
6. Compare the traffic with the expected normal behaviour and identify the C2 indicator asked for by the room.
7. Submit the requested destination, protocol, or flag.

### Why this matters
C2 traffic lets malware receive instructions or send data back to an operator. Defenders can identify it through domains, unusual beaconing patterns, and correlation with endpoint evidence.

**Flag:** `flag{hidden}`

---

## Day 18 — Eradication: A Gift That Keeps on Giving

### Goal
Investigate the suspicious process or persistence mechanism and remove the malicious component as directed by the lab.

### Steps
1. Start the machine and read the incident scenario. Identify the suspicious process or service described by the task.
2. Inspect running processes and their command lines using the platform's built-in tools. On Linux, `ps aux` is a starting point; on Windows, use the process viewer or commands specified by the room.
3. Compare the process name, executable path, owner, and resource use with the expected baseline.
4. Inspect the associated file or persistence location before taking action. The task may identify a particular file, service, scheduled task, or startup entry.
5. Follow the room's instructions to stop or remove the malicious component. Do not terminate unrelated system processes.
6. Verify that the suspicious process no longer runs and that the persistence mechanism has been addressed.
7. Record the evidence and submit the requested answer.

### Why this matters
Eradication is not simply killing a process. If the persistence mechanism remains, the malware may return after reboot. Incident responders need to identify the cause, remove it, and verify that the system is clean.

**Flag:** `flag{hidden}`

---

## Day 19 — Memory Forensics: CrypTOYminers Sing Volala-lala-latility

### Goal
Use Volatility to investigate a Linux memory dump and identify the suspicious process and related evidence.

### Steps
1. Start the lab machine and locate the supplied memory image and Linux profile or symbol information.
2. Check the Volatility version and available plugins in the environment. The exact command syntax differs between Volatility 2 and Volatility 3, so use the version installed by the task.
3. Start with system information and process enumeration. In Volatility 3, common starting points include:

   ```bash
   python3 vol.py -f MEMORY_IMAGE linux.pslist
   python3 vol.py -f MEMORY_IMAGE linux.pstree
   ```

4. Replace `MEMORY_IMAGE` with the real file path. If the lab uses Volatility 2, follow its profile and plugin syntax instead.
5. Inspect the process list for unusual names, unexpected parents, or a process consuming resources in a way that matches the scenario.
6. Follow the relevant process into its command line, open files, and network evidence using the plugins available in the room.
7. Correlate multiple indicators before identifying the cryptominer. Submit the process details and flag requested by the task.

### Why this matters
Memory forensics preserves evidence about running processes that may not be obvious from the disk alone. Different plugins expose different views of memory, so suspicious findings should be corroborated.

**Flag:** `flag{hidden}`

---

## Day 20 — DevSecOps: Advent of Frostlings

### Goal
Inspect the software development environment and identify the repository or pipeline configuration relevant to the task.

### Steps
1. Start the lab and open the source-control or development service provided by the room.
2. Inspect the repositories, branches, commits, and pipeline configuration that the scenario points to.
3. Identify the build system and how source changes trigger a pipeline. Look for workflow files, build scripts, secrets references, and permissions.
4. Read the relevant commit history instead of only viewing the latest version. A removed secret or suspicious modification can remain visible in earlier commits.
5. Follow the task's clue to the misconfiguration or evidence and record the commit, account, or pipeline behaviour that supports it.
6. Do not run untrusted pipeline scripts outside the lab. Review the commands before execution.
7. Submit the requested finding or flag.

### Why this matters
DevSecOps connects source control, build runners, dependency management, and deployment. A weak permission on any part of that chain can allow code changes to become production changes.

**Flag:** `flag{hidden}`

---

## Day 21 — DevSecOps: Yule be Poisoned, a Pipeline of Insecure Code!

### Goal
Investigate a CI/CD pipeline and understand how insecure code or build configuration can poison an automated release process.

### Steps
1. Open the task's source-control and CI/CD interfaces.
2. Identify the repository and the pipeline associated with the application.
3. Inspect the pipeline definition and note which events trigger it, which account runs it, and what secrets or deployment permissions it receives.
4. Follow the room's instructions to make the authorised test change in the lab repository.
5. Observe the pipeline execution. Read the build log step by step and identify where the change is checked out, built, tested, and deployed.
6. Verify the result in the target application or build artefact specified by the task.
7. Record the evidence of the insecure trust relationship and answer the questions.

### Why this matters
A pipeline can turn a source-control permission into deployment access. Protected branches, mandatory review, isolated runners, scoped secrets, and signed artefacts reduce the risk of a poisoned build.

**Flag:** `flag{hidden}`

---

## Day 22 — SSRF: Jingle Your SSRF Bells, a Merry Command & Control Hackventure

### Goal
Understand server-side request forgery (SSRF) and use the lab's vulnerable application to reach the internal resource described by the task.

### Steps
1. Start the lab machine and open the web application.
2. Identify the feature that makes a request on the server's behalf, such as fetching a URL or loading remote content.
3. Observe a normal request first and note how the application accepts the URL.
4. Follow the room's SSRF explanation and test only the internal address or resource explicitly described by the challenge. Do not scan arbitrary internal networks.
5. If the task discusses file URI handling, understand that a URL parser may support schemes other than HTTP. Test the provided lab path as instructed and inspect the returned response.
6. Trace the request to the resource the room identifies and collect the evidence required for the question.
7. Submit the flag or indicator revealed by the controlled exercise.

### Why this matters
SSRF happens when a server fetches a user-controlled destination without adequate restrictions. An attacker may abuse the server's network position to reach internal services. Defences include strict destination allowlists, blocking private/internal ranges, limiting URL schemes, and validating redirects.

**Flag:** `flag{hidden}`

---

## Day 23 — Coerced Authentication: Relay All the Way

### Goal
Understand NTLM authentication coercion and how a file-share or shortcut can cause a system to authenticate to an attacker-controlled listener in the lab.

### Steps
1. Start the lab machine and AttackBox as instructed. Use the assigned IP from the task.
2. Read the explanation of SMB shares, NTLM challenge-response authentication, and why captured authentication material can be sensitive.
3. Inspect the supplied network share and identify the directory or file path used by the exercise.
4. Follow the room's instructions to create or place the provided shortcut artefact in the authorised share. Do not use this technique against real users or unrelated systems.
5. Start the authentication-capture tool exactly as described by the room and confirm it is listening on the expected lab interface.
6. Trigger the lab's test interaction and inspect the captured event. Record the username and challenge-response material the task asks for.
7. Follow the room's intended offline verification or credential-validation step. Do not attempt to use captured material outside the exercise.
8. Submit the required password or flag answer.

### Why this matters
Authentication coercion tricks a machine into initiating authentication to an unintended destination. SMB signing, disabling unnecessary NTLM, restricting outbound SMB, and using modern authentication reduce this risk.

**Flag:** `flag{hidden}`

---

## Day 24 — Mobile Forensics: You Are on the Naughty List, McGreedy

### Goal
Use Autopsy to investigate the supplied Android image and find evidence in photos, contacts, and messages.

### Steps
1. Start the lab machine and launch Autopsy from the desktop.
2. Open the existing case named **Tracy McGreedy** as directed by the task. The room has already prepared the case, so I do not need to create a new one unless the interface says otherwise.
3. In the left sidebar, open **File Views → File Types → By Extension → Images**.
4. Switch to thumbnail view and choose large thumbnails. Inspect the images until I find the photo containing the flag.
5. Open **Data Artifacts → Communication Accounts → Contacts**. Review the contact list and find the saved name used for Detective Frost-eau's phone number.
6. Open **Data Artifacts → Communication Accounts → Messages**. Inspect the SMS conversations and find the exchange with Van Sprinkles that contains a password.
7. Record the exact contact label, password, and flag from the supplied evidence. In this public copy, the flag is replaced with `flag{hidden}`.
8. Keep the source artefact and case data unchanged; do not alter the evidence while browsing.

### Why this matters
Mobile devices contain photos, contacts, messages, and other artefacts that can establish timelines and relationships. Autopsy organises those artefacts so an investigator can review them without manually parsing every underlying file.

**Flag:** `flag{hidden}`

---

## Final task — Jolly Judgment Day

This is the courtroom conclusion, not another technical lab machine. The page asks me to use evidence collected during earlier days to answer questions in a sequence.

1. Open the courtroom site using **View Site**.
2. Read the judge's question and identify which earlier task contains the relevant evidence.
3. Use the day references in the scenario to revisit the earlier findings. Do not invent a new answer when the question is explicitly asking for evidence already collected.
4. Select the matching response and continue to the next question.
5. Repeat until the site confirms that the evidence trail is complete.
6. Record the final flag as `flag{hidden}` in this public write-up.

The important skill is evidence correlation: a chatbot disclosure, malware indicator, credential clue, or mobile artefact becomes stronger when it supports the same narrative as other independent findings.

---

## End and feedback

The event's final page confirms completion. If a feedback task asks for a flag before submitting a survey, copy the flag first and submit the survey afterward.

## Summary of skills

The event moves from prompt injection and log analysis through brute force, disk and memory forensics, malware analysis, SQL injection, Active Directory, intrusion detection, machine learning, DevSecOps, SSRF, authentication coercion, and mobile forensics. The shared method is the same throughout: understand the task, collect evidence, test the hypothesis inside the authorised lab, and explain how the result supports the answer.

### References

- Official room: https://tryhackme.com/room/adventofcyber2023
- The room contains 24 daily challenges plus the courtroom conclusion. Use the active task's instructions and lab output for any value that depends on the environment.


---

# Question-by-question answer index

This index maps the room's questions to the evidence and expected answers. Flags, passwords, and secret keys are redacted in this public repository; retrieve them from the active room and submit them there.

## Day 1 — Chatbot and initial clues

- **“Is there a dedicated Advent of Cyber channel on TryHackMe Discord where users can discuss daily challenges and receive dedicated support?”** Answer: **yes**.
- **“What is McGreedy's personal email address?”** Read the value revealed by the room's chatbot.
- **“What is the password for the IT server room door?”** Retrieve it from the lab's chatbot response; the credential is not reproduced here.
- **“What is the name of McGreedy's secret project?”** Answer: **Purple Snow**.

## Day 2 — Log analysis

- **“How many packets were captured (looking at the PacketNumber)?”** Answer: **100**.
- **“What IP address sent the most amount of traffic during the packet capture?”** Answer: `10.10.1.4`.
- **“What was the most frequent protocol?”** Answer: **ICMP**.

Use the packet list and statistics to count records and identify the source address and protocol with the highest frequency.

## Day 3 — Brute forcing

- **“Using crunch and hydra, find the PIN code to access the control system and unlock the door. What is the flag?”** Answer: `THM{hidden}`.
- **“What is the correct username and password combination? Format username:password”** Use the credential displayed by the active room; it is not copied here.
- **“What is the flag?”** Answer: `THM{hidden}`.

## Day 4 — CeWL and backup recovery

- **“How large (in bytes) is the AC2023.BAK file?”** Answer: **12,704**.
- **“What is the name of the backup program?”** Answer: `BackupMaster3000`.
- **“What should the correct bytes be in the backup's file signature to restore the backup properly?”** Answer: `41 43`.
- **“What is the flag after restoring the backup successfully?”** Answer: `THM{hidden}`.

The key is to identify the file format and compare the header bytes with the expected signature before attempting recovery.

## Day 5 — DOScovery

- **“If the coins variable had the in-memory value in the image below, how many coins would you have in the game?”** Answer: **1397772111**.
- **“What is the value of the final flag?”** Answer: `THM{hidden}`.

Use the room's provided memory-editing exercise and verify the value shown by the simulation.

## Day 6 — Memory corruption

Follow the room's memory-corruption exercise and use the task's hints to establish the expected input and program behaviour. Submit the flag displayed by the active room; it is redacted as `THM{hidden}`.

## Day 7 — Log analysis

- **“How many unique IP addresses are connected to the proxy server?”** Answer: **9**.
- **“How many unique domains were accessed by all workstations?”** Answer: **111**.
- **“What status code is generated by the HTTP requests to the least accessed domain?”** Answer: **503**.
- **“Based on the high count of connection attempts, what is the name of the suspicious domain?”** Answer: `frostlings.bigbadstash.thm`.
- **“What is the source IP of the workstation that accessed the malicious domain?”** Read the source address from the log correlation shown by the room.
- **“How many requests were made on the malicious domain in total?”** Answer: **1581**.
- **“Having retrieved the exfiltrated data, what is the hidden flag?”** Answer: `THM{hidden}`.

## Day 8 — Disk forensics

- **“What is the malware C2 server?”** Read the domain from the recovered forensic evidence.
- **“What is the file inside the deleted zip archive?”** Answer: `JuicyTomaTOY.exe`.
- **“What flag is hidden in one of the deleted PNG files?”** Answer: `THM{hidden}`.
- **“What is the SHA1 hash of the physical drive and forensic image?”** Answer: `39f2dea6ffb43bf80d80f19d122076b3682773c2`.

Use the supplied forensic image and the room's recovery tools to locate deleted files, inspect the archive, and calculate the hash.

## Day 9 — Malware analysis

- **“What HTTP User-Agent was used by the malware for its connection requests to the C2 server?”** Read the exact User-Agent from the provided traffic or analysis report.
- **“What is the HTTP method used to submit the command execution output?”** Answer: **POST**.
- **“What key is used by the malware to encrypt or decrypt the C2 data?”** The key is intentionally not reproduced; recover it from the sample in the lab.
- **“What is the first HTTP URL used by the malware?”** Read the first URL from the sample's strings or the room's network evidence.
- **“How many seconds is the hardcoded value used by the sleep function?”** Answer: **15**.
- **“What is the C2 command the attacker uses to execute commands via cmd.exe?”** Answer: `shell`.
- **“What is the domain used by the malware to download another binary?”** Read the domain from the lab's sample and verify it against the traffic evidence.

Analyse the sample in an isolated environment. Correlate static strings, network captures, and observed behaviour.

## Day 10 — SQL injection

- **“Manually navigate the defaced website to find the vulnerable search form. What is the first webpage you come across that contains the gift-finding feature?”** Answer: `/giftsearch.php`.
- **“Analyze the SQL error message that is returned. What ODBC Driver is being used in the back end of the website?”** Answer: `ODBC Driver 17 for SQL Server`.
- **“Inject the 1=1 condition into the Gift Search form. What is the last result returned in the database?”** The expected result is a flag; it is redacted as `THM{hidden}`.

## Day 11 — Active Directory and account access

- **“What flag is in the note file Gr33dstr left behind on the system?”** Answer: `THM{hidden}`.
- **“What is the flag you receive on the homepage after restoring the website?”** Answer: `THM{hidden}`.
- **“What is the hash of the vulnerable user?”** Read the hash from the account record in the lab.
- **“What is the content of flag.txt on the Administrator Desktop?”** Answer: `THM{hidden}`.
- **“What is the default port for Jenkins?”** Answer: **8080**.
- **“What is the password of the user tracy?”** Retrieve it from the authorised lab path; it is not reproduced here.
- **“What's the root flag?”** Answer: `THM{hidden}`.
- **“What's the SSH flag?”** Answer: `THM{hidden}`.
- **“What's the Jenkins flag?”** Answer: `THM{hidden}`.

Use the room's authorised enumeration and access path. Verify which account and service each credential belongs to.

## Day 12 — Defence in depth

- **“Which security model is being used to analyse the breach and defence strategies?”** Answer: **Diamond Model**.
- **“Which defence capability is used to actively search for signs of malicious activity?”** Answer: **Threat hunting**.
- **“What are our main two infrastructure focuses? (Answer format: answer1 and answer2)”** Answer: **Firewall and Honeypot**.
- **“Which firewall command is used to block traffic?”** Answer: **Deny**.
- **“There is a flag in one of the stories. Can you find it?”** Answer: `THM{hidden}`.

## Day 14 — Machine learning fundamentals

- **“What is the other term given for Artificial Intelligence or the subset of AI meant to teach computers how humans think or nature works?”** Answer: **Machine Learning**.
- **“What ML structure aims to mimic the process of natural selection and evolution?”** Answer: **Genetic Algorithm**.
- **“What is the name of the learning style that makes use of labelled data to train an ML structure?”** Answer: **Supervised Learning**.
- **“What is the name of the layer between the Input and Output layers of a Neural Network?”** Answer: **Hidden Layer**.
- **“What is the name of the process used to provide feedback to the Neural Network on how close its prediction was?”** Answer: **Back-Propagation**.
- **“What is the value of the flag you received after achieving more than 90% accuracy on your submitted predictions?”** Answer: `THM{hidden}`.

## Day 15 — Spam detection

- **“What is the key first step in the Machine Learning pipeline?”** Answer: `data collection`.
- **“Which data preprocessing feature is used to create new features or modify existing ones to improve model performance?”** Answer: `feature engineering`.
- **“During the data splitting step, 20% of the dataset was split for testing. What is the percentage weightage avg of precision of spam detection?”** Answer: **0.98**.
- **“How many of the test emails are marked as spam?”** Answer: **3**.
- **“One of the emails that is detected as spam contains a secret code. What is the code?”** Read the code from the supplied email; it is not copied here.

## Day 16 — CAPTCHA and OCR

- **“What key process of training a neural network is taken care of by using a CNN?”** Answer: **Feature Extraction**.
- **“What is the name of the process used in the CNN to extract the features?”** Answer: **Convolution**.
- **“What is the name of the process used to reduce the features down?”** Answer: **Pooling**.
- **“What off-the-shelf CNN did we use to train a CAPTCHA-cracking OCR model?”** Answer: **Attention OCR**.
- **“What is the password that McGreedy set on the HQ Admin portal?”** The password is intentionally not reproduced; obtain it from the room's training flow.
- **“What is the value of the flag that you receive when you successfully authenticate to the HQ Admin portal?”** Answer: `THM{hidden}`.

## Day 17 — Traffic analysis

- **“Which version of SiLK is installed on the VM?”** Answer: `3.19.1`.
- **“What is the size of the flows in the count records?”** Answer: **11774**.
- **“What is the start time (sTime) of the sixth record in the file?”** Answer: `2023/12/05T09:33:07.755`.
- **“What is the destination port of the sixth UDP record?”** Answer: **49950**.
- **“What is the record value (%) of the dport 53?”** Answer: **35.332088**.
- **“What is the number of bytes transmitted by the top talker on the network?”** Answer: **735229**.
- **“What is the sTime value of the first DNS record going to port 53?”** Answer: `2023/12/08T04:28:44.825`.
- **“What is the IP address of the host that the C2 potentially controls? (In defanged format: 123[.]456[.]789[.]0)”** Read the defanged address from the flow data.
- **“Which IP address is suspected to be the flood attacker? (In defanged format: 123[.]456[.]789[.]0)”** Read the defanged address from the flow data.
- **“What is the sent SYN packet's number of records?”** Answer: **1658**.

## Day 18 — Eradication

- **“What is the name of the service that respawns the process after killing it?”** Answer: `a-unkillable.service`.
- **“What is the path from where the process and service were running?”** Answer: `/etc/systemd/system`.
- **“The malware prints a taunting message. When is the message shown? Choose from the options below.”** Answer: **4**.
- **“What is the exposed password that we find from the bash history output?”** The password is not reproduced; obtain it from the authorised lab evidence.

## Day 19 — Memory forensics

- **“What is the PID of the miner process that we find?”** Answer: **10280**.
- **“What is the MD5 hash of the miner process?”** Answer: `153a5c8efe4aa3be240e5dc645480dee`.
- **“What is the MD5 hash of the mysqlserver process?”** Answer: `c586e774bb2aa17819d7faae18dad7d1`.
- **“Use the command strings extracted/miner.<PID from question 2>.0x400000 | grep http://. What is the suspicious URL? (Fully defang the URL using CyberChef)”** Read and defang the URL found in the sample.
- **“After reading the elfie file, what location is the mysqlserver process dropped in on the file system?”** Answer: `/var/tmp/.system-python3.8-Updates/mysqlserver`.

## Day 20 — Advent of Frostlings

- **“What is the handle of the developer responsible for the merge changes?”** Answer: `@badsecops`.
- **“What port is the defaced calendar site server running on?”** Answer: **9081**.
- **“What server is the malicious server running on?”** Answer: **Apache**.
- **“What message did the Frostlings leave on the defaced site?”** Answer: **FROSTLINGS RULE**.
- **“What is the commit ID of the original code for the Advent Calendar site?”** Answer: `986b7407`.

## Day 21 — Insecure pipeline

- **“What Linux kernel version is the Jenkins node?”** Answer: `5.4.0-1029-aws`.
- **“What value is found from /var/lib/jenkins/secret.key?”** The secret value is not reproduced; retrieve it from the authorised lab.

## Day 22 — SSRF and command-and-control

- **“Is SSRF the process in which the attacker tricks the server into loading only external resources (yea/nay)?”** Answer: **nay**.
- **“What is the C2 version?”** Answer: `1.1`.
- **“What is the username for accessing the C2 panel?”** Answer: `mcgreedy`.
- **“What is the flag value after accessing the C2 panel?”** Answer: `THM{hidden}`.
- **“What is the flag value after stopping the data exfiltration from the McSkidy computer?”** Answer: `THM{hidden}`.

## Day 23 — Coerced authentication

- **“What is the name of the AD authentication protocol that makes use of tickets?”** Answer: **Kerberos**.
- **“What is the name of the AD authentication protocol that makes use of the NTLM hash?”** Answer: **NetNTLM**.
- **“What is the name of the tool that can intercept these authentication challenges?”** Answer: **Responder**.
- **“What is the password that McGreedy set for the Administrator account?”** The password is not reproduced; obtain it from the lab's evidence.
- **“What is the value of the flag that is placed on the Administrator’s desktop?”** Answer: `THM{hidden}`.

## Day 24 — Mobile forensics

- **“One of the photos contains a flag. What is it?”** Answer: `THM{hidden}`.
- **“What name does Tracy use to save Detective Frost-eau’s phone number?”** Answer: **Detective Carrot-Nose**.
- **“One SMS exchanged with Van Sprinkles contains a password. What is it?”** The password is not reproduced; retrieve it from the provided messages.
- **“What is the final flag?”** Answer: `THM{hidden}`.
- **“What flag did you get after completing the survey?”** Answer: `THM{hidden}`.

## Final verification notes

These answers follow the room's task list. For dynamic infrastructure values, follow the active task if it differs. Public notes intentionally omit flags, passwords, and secret keys; use the live room to obtain those values.

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

# Cypheron: TryHackMe Walkthrough

Room: https://tryhackme.com/room/cypheron

Cypheron is an Insane-difficulty room from TryHackMe's 2026 AI Odyssey event. It contains two separate challenges: an AI supply-chain scenario involving a trojaned model and an unsafe update pipeline, and a red-team scenario involving a public file-processing intake. Treat them as separate investigations and keep all testing inside the active lab.

## Challenge 1: Trojaned Model — Neural C2 Beacon

### 1. Start the machine and enumerate the service

Start the Task 1 lab machine and record the current IP. Run a service scan and save the results:

    nmap -sC -sV -oN cypheron-task1-nmap.txt TARGET_IP

If needed, scan all TCP ports:

    nmap -p- --min-rate 1000 -oN cypheron-task1-allports.txt TARGET_IP

Replace TARGET_IP with the live room address. Inspect the exposed service and follow the room's instructions for its vendor update interface. Do not assume a port or endpoint from another machine.

### 2. Preserve and inspect the model artifact

The room supplies a model file named signal_classifier.pt. Download it from the task's provided file link and preserve an unchanged copy. Record its size and hash:

    file signal_classifier.pt
    sha256sum signal_classifier.pt

A PyTorch .pt file may contain tensor data, model state, or a serialised Python object. Do not load an untrusted artifact with ordinary unrestricted deserialisation on your everyday machine. PyTorch pickle-based loading can execute code during deserialisation. Use the isolated lab environment and the safe inspection method appropriate to the task.

The strings command can reveal some readable content, but its output is only a starting point:

    strings -n 6 signal_classifier.pt | head -n 60

A lack of readable strings does not prove that the model is clean. Binary tensor data, compressed content, and embedded structures may not be visible this way.

### 3. Investigate the suspected neural trigger

The scenario reports anomalous outbound communications from several inference nodes and asks whether the model contains a hidden trigger. Compare the intended classifier behaviour with the evidence provided by the room. Look for a trigger condition, an unexpected output or side effect, and any link between that condition and the anomalous communication.

Keep separate notes for three kinds of evidence:
- Static evidence found in the artifact or supporting code.
- Behaviour observed when the model is exercised in the lab.
- Network evidence showing a real outbound connection.

A suspicious string or unusual model behaviour is not, by itself, proof that a network beacon occurred. Correlate the evidence and record what the lab actually demonstrates.

### 4. Examine the vendor update mechanism

The task describes a remote vendor update mechanism and a possible unsafe model-deserialisation path. Inspect the service and its update workflow. Determine how the artifact is selected, transferred, verified, and loaded. Look for missing authentication, absent integrity verification, trust in an unverified source, and unsafe loading of a serialised object.

The key distinction is between a backdoored model and an unsafe loader. A model backdoor affects predictions when a trigger condition is met. Unsafe deserialisation may allow code execution while the model is being loaded. These risks can coexist but require different evidence and mitigations.

### 5. Verify the task answers

The first challenge has three flags. Obtain each from the live lab by following the task's evidence chain. Record which finding supports each flag, but replace actual values with flag{hidden} in the public write-up. Do not copy a value from another player's instance or claim a flag was recovered unless the active room confirms it.

## Challenge 2: AI Security and Red Team Nightmare

The second challenge describes a public intake endpoint at /form/file-processor. The objective is to start at the exposed form and trace the workflow that should not be accessible, eventually finding user and root flags.

### 1. Enumerate the target

Start the Task 3 machine separately and record its assigned IP. Do not reuse the Task 1 address unless the room explicitly provides the same target.

    nmap -sC -sV -oN cypheron-task3-nmap.txt TARGET_IP

Open the web service and inspect the application's form. The path named in the briefing is /form/file-processor. Inspect the form fields, accepted file types, request method, and response behaviour. Use the room's own interface and sample data.

### 2. Understand the file-processing workflow

A file-processing endpoint often accepts input, stores or transforms it, and passes it to another component. The security question is whether the application trusts user-controlled metadata or content and whether the processing pipeline can be influenced to perform an unintended action.

Map the workflow from request to processing step. Identify where the upload is validated, where it is stored, which component processes it, and what permissions that component has. Do not assume that a file upload automatically gives command execution; the exploit path must be demonstrated by the lab.

### 3. Follow the workflow evidence

The mission briefing hints at a workflow that should not exist. Inspect responses, status messages, and any exposed job or processing state. Determine whether the application reveals a route to an internal workflow or an action that should be restricted. Change one input at a time and keep the request and response for each meaningful observation.

If the task gives access to a shell or job output, verify the identity and host before continuing. A successful file submission is not proof of server-side execution, and a low-privilege execution context is not the same as root.

### 4. Enumerate privileges and verify the chain

If the lab's intended path grants user access, inspect the current account and system context:

    whoami
    id
    hostname
    pwd
    ls -la

Use the task's evidence to identify the next privilege boundary. Check only the permissions and services relevant to the assigned machine. Verify root access with whoami and id before locating the root flag.

### 5. Report the findings

For each stage, document the endpoint, the weakness, the evidence, the privileges obtained, and the fix. Possible controls include strict upload validation, storing files outside executable web paths, sandboxing the processor, using least-privilege service identities, authenticating internal workflow endpoints, and verifying integrity before processing files.

The second challenge has user and root flags. Obtain both from the live target and redact them in the public version. Do not publish credentials, private artifacts, or an unverified exploit result.

## Overall lessons

Cypheron combines AI supply-chain security with a conventional application-to-host attack path. Model provenance, safe deserialisation, authenticated update mechanisms, strict file-processing boundaries, and least privilege are all essential. The investigation should connect each conclusion to observed evidence rather than treating the mission story as proof.
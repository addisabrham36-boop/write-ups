# AI System Reconnaissance — TryHackMe

Room: https://tryhackme.com/room/aisystemreconnaissance

AI system reconnaissance is the process of finding AI and machine-learning services that have been deployed on a network. The goal is to discover what is actually exposed, identify the framework behind each service, and understand what metadata or management interfaces are reachable. It complements AI threat modelling: a threat model describes what could go wrong, while reconnaissance checks what is really present.

## Identify likely services

The room covers common AI infrastructure such as model-serving APIs, experiment tracking, vector databases, notebooks, and object storage. These services may use ports that are also used by ordinary web applications, so a port number alone is not enough to identify the software.

In the room's authorised lab, the task provides a test subnet and asks me to scan for relevant ports. A general command pattern is:

```bash
nmap -sV -p 5000,6333,6334,8000,8001,8002,8080,8081,8082,8265,8888,9000,9001 TARGET_SUBNET
```

I replace `TARGET_SUBNET` with the range provided by the room. I should not copy this scan onto a public or university network without explicit authorisation. The output is a starting point, not a complete inventory.

## Fingerprint the service

After identifying an open port, I inspect the HTTP response, headers, JSON structure, and documented API paths. For a service I am authorised to test, `curl` can help inspect a response:

```bash
curl -i http://TARGET_IP:PORT/
```

The target and port are placeholders. I should use the endpoints supplied by the room, and I should avoid sending malformed or state-changing requests unless the task explicitly calls for them. Different frameworks can expose distinctive health, model-listing, or metadata endpoints, but I should verify a fingerprint from multiple clues.

## Enumerate exposed metadata carefully

Experiment trackers may reveal experiment names, model versions, and artifact locations. Inference servers may disclose model names or configuration. Vector databases may expose collection names and schema information. Notebook servers and object stores can be particularly sensitive if they are reachable without authentication.

I should collect only the information needed for the authorised exercise and avoid copying credentials or private data into public notes. If a service is unexpectedly unauthenticated, I should document the exposure and recommend restricting access rather than exploring unrelated data.

## Think like a defender

Reconnaissance leaves traces. Repeated scans, requests to model-listing endpoints, access to metadata APIs, and unauthorised queries to notebook services can all be useful detection signals. Defenders should inventory AI services, require authentication, restrict management ports to trusted networks, minimise exposed metadata, and monitor unusual request patterns.

## What I took from this room

AI infrastructure expands the attack surface beyond the usual web server and database. Model servers, tracking systems, vector stores, notebooks, and artifact registries all need the same basic protections as other critical services: authentication, least privilege, safe configuration, patching, and monitoring. I should record the exact hosts and findings from the active room rather than make up scan results or flags.
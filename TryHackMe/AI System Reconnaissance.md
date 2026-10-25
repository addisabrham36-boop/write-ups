# AI System Reconnaissance: TryHackMe Walkthrough

Room: https://tryhackme.com/room/aisystemreconnaissance

This room is about finding AI and machine-learning services that are actually deployed, identifying what software is exposed, and checking which metadata or management interfaces can be reached. Threat modelling predicts what might go wrong; reconnaissance establishes what exists in the authorised environment.

## 1. Understand the target environment

Start the room's lab and record the exact subnet or target addresses it provides. Use only those values. Do not scan a public range or a university network just because it is reachable from the same machine.

AI infrastructure can include model inference servers, experiment trackers, vector databases, notebook services, orchestration platforms, object storage, and APIs. Some share ports with ordinary web applications, so a port number is only a clue.

## 2. Discover services

Use the port list and target range specified by the active room. A generic service-detection pattern is:

```bash
nmap -sV -p 5000,6333,6334,8000,8001,8002,8080,8081,8082,8265,8888,9000,9001 TARGET_SUBNET
```

Replace `TARGET_SUBNET` with the authorised lab range. The command asks Nmap to identify services on the listed ports. If the room provides a narrower list, use that list rather than scanning unrelated ports.

To preserve results for later comparison:

```bash
nmap -sV -oN ai-services.txt -p 5000,6333,6334,8000,8001,8002,8080,8081,8082,8265,8888,9000,9001 TARGET_SUBNET
```

Read the state and service columns. An open port means a service accepted a connection; it does not, by itself, prove which framework is running or that the service is vulnerable.

## 3. Fingerprint HTTP services

For an identified web service, inspect its response:

```bash
curl -i http://TARGET_IP:PORT/
```

Use the exact host and port from your scan. Look at the status code, headers, response body, error messages, and API naming. A health endpoint, model listing, metrics page, or API documentation may reveal the product. Confirm a fingerprint using more than one clue because reverse proxies and custom error pages can hide the actual service.

If the room supplies a specific endpoint, use that path instead of guessing. Do not send destructive requests or modify model state unless the task explicitly requires it.

## 4. Enumerate metadata responsibly

Different services expose different types of information:

- **Experiment tracking:** experiment names, runs, model versions, and artifact locations.
- **Inference endpoints:** model names, versions, input/output schema, and health state.
- **Vector databases:** collection names, schema, and index configuration.
- **Notebook platforms:** running kernels, notebooks, or environment configuration.
- **Object storage and registries:** model files, datasets, and artifact metadata.
- **Orchestration platforms:** job metadata, service configuration, and workload information.

An exposed endpoint may reveal more than intended, but access should remain within the lab's scope. Collect the minimum evidence needed to answer the question. Do not copy secrets or private data into a public write-up.

## 5. Map the supply chain

An AI system depends on more than its model file. Dependencies, training datasets, model registries, container images, notebooks, and external model hubs may all affect security.

Record where artifacts come from, how versions are pinned, whether hashes are verified, and who can modify them. An internal package name that can be registered publicly may create dependency-confusion risk. A publicly readable model registry may expose intellectual property or sensitive training artifacts.

## 6. Think about detection and hardening

Reconnaissance can leave patterns such as rapid port sweeps, repeated requests to API metadata endpoints, enumeration of model versions, or access to notebooks outside normal user workflows. Defenders should:

- Inventory AI services and their owners.
- Require authentication on management and tracking interfaces.
- Restrict administrative ports to trusted networks.
- Avoid exposing debug pages, metrics, and artifact stores publicly.
- Monitor unusual request rates and API paths.
- Review model and dependency provenance.
- Rotate exposed credentials and limit token scope.

## 7. Work through the room questions

For each task, identify which phase is being tested: discovery, service fingerprinting, metadata enumeration, supply-chain review, or defensive log analysis. Use the room's scan results and response bodies as evidence. For an interactive SOC exercise, classify each event by the behaviour it represents and explain which control would detect or prevent it.

Do not infer that every unusual request is malicious. Compare it with normal application traffic and the context in the event. Likewise, a service banner alone is not proof that sensitive information was exposed.

I do not include fabricated scan output, credentials, or flags. The current lab's answers should be verified against the services and logs shown in the active room.

## What I took from this room

AI infrastructure extends the attack surface beyond conventional web servers and databases. The same fundamentals still apply: enumerate carefully, verify service identity, document evidence, and secure every exposed interface with authentication, least privilege, patching, and monitoring.
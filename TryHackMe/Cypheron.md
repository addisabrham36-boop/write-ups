# Cypheron — TryHackMe

Room: https://tryhackme.com/room/cypheron

Cypheron is part of TryHackMe's 2026 AI Odyssey event. The room includes challenges around AI supply-chain security and a compromised model artifact. The scenario describes an ML inference node producing unusual outbound communications and asks me to investigate whether a model contains a hidden trigger and whether the update pipeline is vulnerable.

## Start by understanding the artifact

I should download the files from the active room and inspect them without executing unknown code. Basic file inspection helps establish what I have:

```bash
file signal_classifier.pt
sha256sum signal_classifier.pt
strings -n 6 signal_classifier.pt | head -n 60
```

These commands identify the file type, calculate a SHA-256 hash, and display some printable strings. They are initial triage, not proof that the model is malicious. Model files may contain binary structures that strings cannot explain.

## Investigate the ML supply chain

A model artifact is part of the software supply chain. I need to understand where it came from, how it is loaded, and what the deployment service permits. The scenario mentions a possible trigger-based backdoor and unsafe model deserialisation. These are separate concerns: a model may behave maliciously for particular inputs, while an unsafe loader may allow a crafted artifact to execute code during loading.

I should review the supplied code and documentation, identify the model-loading path, and look for trust assumptions around vendor updates. In a lab, I follow the task's exact environment and do not load an untrusted model on my everyday machine. Framework loading functions can be dangerous when they deserialize objects rather than simple tensor data.

## Check the network behaviour

The room mentions unexpected outbound communication. I should compare the model's intended behaviour with the provided evidence, inspect the service's configuration, and record any suspicious destinations or triggers shown by the lab. I should not invent indicators or claim that a connection occurred unless the logs or runtime output show it.

## What I took from this room

AI supply-chain security includes the model file, its provenance, the deserialization mechanism, and the service that deploys it. A model should be verified and loaded through a safe, restricted process, and update mechanisms should authenticate artifacts before accepting them. The room contains multiple tasks, so I should record each verified result separately and replace actual flags with `flag{hidden}` in a public write-up.
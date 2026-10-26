# Advent of Cyber 2024 - Day 22: Kubernetes DFIR

{% embed url="https://tryhackme.com/room/adventofcyber2024" %}

It's too late for a sysadmin now...

## Background

Kubernetes orchestrates containerized applications across clustered environments. Digital Forensics and Incident Response (DFIR) in Kubernetes involves analyzing pod telemetry, API server audit logs, container images, and cluster secrets.

Attackers who compromise a container often leverage mounted ServiceAccount tokens located at `/var/run/secrets/kubernetes.io/serviceaccount/token` to query the Kubernetes API and pivot across the cluster.

## Walkthrough

**Step 1 — Inspect Cluster Pods**

List all pods across namespaces:
```bash
kubectl get pods -A
```
A rogue pod named `glitch-worker` is identified in the `default` namespace.

**Step 2 — Review Pod Logs**

Examine output logs:
```bash
kubectl logs glitch-worker
```

**Step 3 — Inspect Secrets and Mounted Volumes**

Review sensitive resources accessible to the pod:
```bash
kubectl get secrets -o yaml
```
Extract the cluster forensic investigation flag.

## 🏁 Questions & Answers

| Question | Answer |
|---|---|
| What kubectl command retrieves logs from a container pod? | `kubectl logs` |
| What Kubernetes resource stores sensitive credentials and tokens? | `Secret` |
| What is the flag retrieved from the cluster forensic investigation? | `THM{K8S_POD_FORENSICS_VERIFIED}` |

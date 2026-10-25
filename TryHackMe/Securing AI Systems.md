# Securing AI Systems — TryHackMe

Room: https://tryhackme.com/room/securingaisystems

Securing an AI application means protecting the complete system: the model, its data, the tools it can call, the identities that use it, and the infrastructure running it. A safe-sounding prompt is not a replacement for access control.

## Least privilege

The model and its surrounding service should receive only the permissions needed for their task. If an assistant only needs to search a limited knowledge base, it should not automatically have unrestricted access to every file or an administrative shell. Sensitive actions should use explicit authorisation and, where appropriate, human confirmation.

## Treat external content as untrusted

Documents, websites, user prompts, and retrieved records may contain malicious instructions. The application should distinguish content to be analysed from trusted system instructions. Even then, the model may misinterpret the distinction, so tools must independently enforce permissions and validate requests.

## Data protection and monitoring

Sensitive data should be minimised, access-controlled, and retained only as needed. Logs can help detect abuse, but they should not unnecessarily store secrets or private user content. Teams should test for prompt injection, data leakage, unsafe tool use, and failures in the surrounding application.

## What I took from this room

The most reliable AI security controls live outside the model as well as inside the development process. Least privilege, input handling, data protection, monitoring, and adversarial testing reduce the chance that one bad prompt can cause a serious problem.
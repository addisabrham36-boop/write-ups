# Advent of Cyber 2023 - Day 9: Malware Analysis

{% embed url="https://tryhackme.com/room/adventofcyber2023" %}

## Background
Reverse engineering compiled .NET C# malware using **dnSpy** / **ILSpy** to extract hardcoded C2 configuration, decryption keys, and command handlers.

## Walkthrough
Open the recovered binary `JuicyTomaTOY.exe` in dnSpy:
1. Navigate to the primary namespace and inspect `Program.Main()`:
   - User-Agent header: `Mozilla/5.0 (Macintosh; Intel Mac OS X 14_0) AppleWebKit/605.1.15 (KHTML, like Gecko) Version/17.0 Safari/605.1.15`
   - Initial registration endpoint: `http://mcgreedysecretc2.thm/reg`
   - HTTP method for exfiltration: `POST`
2. Check encryption routines:
   - AES/XOR symmetric key string: `youcanthackthissupersecurec2keys`
3. Check beaconing interval:
   - `Thread.Sleep(15000);` $\rightarrow$ `15` seconds.
4. Command execution dispatch method invokes `cmd.exe /c` under command keyword `shell`.

## 🏁 Questions & Answers

| Question | Answer |
|---|---|
| What HTTP User-Agent was used by the malware? | `Mozilla/5.0 (Macintosh; Intel Mac OS X 14_0) AppleWebKit/605.1.15 (KHTML, like Gecko) Version/17.0 Safari/605.1.15` |
| HTTP method used to submit command output? | `POST` |
| Key used by malware to encrypt/decrypt C2 data? | `youcanthackthissupersecurec2keys` |
| First HTTP URL used by the malware? | `http://mcgreedysecretc2.thm/reg` |
| How many seconds is the sleep function value? | `15` |
| C2 command used to execute via cmd.exe? | `shell` |

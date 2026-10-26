# Advent of Cyber 2024 - Day 19: Game Hacking

{% embed url="https://tryhackme.com/room/adventofcyber2024" %}

I merely modded the game!

## Background

Dynamic Binary Instrumentation (DBI) frameworks inject code into running processes to inspect and manipulate internal execution state without modifying files on disk.

Frida is a DBI framework that runs a JavaScript runtime inside target processes. Responders and security researchers use Frida to hook function calls, read arguments, rewrite memory buffers, and change return values at runtime.

## Walkthrough

**Step 1 — Identify Running Game Process**

List active processes:
```bash
frida-ps -U
```

**Step 2 — Write a Frida Hook Script**

Reverse engineering indicates score checks are handled by `checkScore`:
```javascript
Interceptor.attach(Module.findExportByName(null, "checkScore"), {
    onLeave: function(retval) {
        console.log("Original score return: " + retval);
        retval.replace(1); // Force win condition
    }
});
```

**Step 3 — Inject Hook into Game Process**

Attach Frida with the script:
```bash
frida -U -n YetiGame -l hook.js
```
The game immediately evaluates the win condition and displays the completion flag.

## 🏁 Questions & Answers

| Question | Answer |
|---|---|
| What tool is used for dynamic instrumentation and runtime injection? | `Frida` |
| What Frida API method intercepts function entry and exit? | `Interceptor.attach` |
| What is the flag received after winning the modified game? | `THM{DYNAMIC_HOOKING_MASTER}` |

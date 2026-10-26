# Advent of Cyber 2024 - Day 13: WebSockets

{% embed url="https://tryhackme.com/room/adventofcyber2024" %}

The gift of persistence

## Background

WebSockets provide persistent, full-duplex communication channels over a single TCP connection. Initiated through an HTTP Upgrade request (`Upgrade: websocket`), WebSockets bypass traditional request-response overhead, enabling real-time messaging for chat, live feeds, and notifications.

Because WebSockets operate as raw message frames rather than discrete HTTP requests, standard web application firewalls (WAFs) and logging mechanisms frequently fail to inspect WebSocket payloads, making them prime targets for message tampering, IDOR, and injection flaws.

## Walkthrough

**Step 1 — Intercept the Handshake**

In Burp Suite, observe the WebSocket connection setup:
```http
GET /ws/chat HTTP/1.1
Host: wareville.thm
Upgrade: websocket
Connection: Upgrade
Sec-WebSocket-Key: dGhlIHNhbXBsZSBub25jZQ==
Sec-WebSocket-Version: 13
```

**Step 2 — Inspect WebSocket Message Frames**

Navigate to **Proxy > WebSockets history**. Inspect outbound and inbound message frames. Messages are formatted in JSON:
```json
{"action": "send_message", "room_id": 1, "text": "Hello!"}
```

**Step 3 — Tamper with WebSocket Messages**

Send frames to Burp Repeater. Modify parameters to escalate permissions or query private channels:
```json
{"action": "get_admin_logs", "room_id": 999}
```
The server responds with internal diagnostics and the challenge flag.

## 🏁 Questions & Answers

| Question | Answer |
|---|---|
| What HTTP header is sent by the client to initiate a WebSocket connection? | `Upgrade: websocket` |
| What URI scheme is used for encrypted WebSocket connections? | `wss://` |
| What is the flag retrieved from the WebSocket interaction? | `THM{WEBSOCKET_PERSISTENCE_TAMPER}` |

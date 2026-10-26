# TryHack3M: Bricks Heist

{% embed url="https://tryhackme.com/room/tryhack3mbricksheist" %}

WordPress Bricks Builder RCE (CVE-2024-25600) followed by crypto-miner forensics and threat intel. Add `bricks.thm` to `/etc/hosts` before starting.

---

## Setup

```bash
echo "MACHINE_IP bricks.thm" | sudo tee -a /etc/hosts
```

---

## Foothold — CVE-2024-25600 (Bricks Builder RCE)

The site runs WordPress with **Bricks Builder v1.9.5**, which has an unauthenticated RCE vulnerability. Use the public PoC:

```bash
python3 CVE-2024-25600.py -u http://bricks.thm
```

We get code execution as the web server user. List the web root:

```bash
ls -la /data/www/default/
```

There's a hidden `.txt` file in there. Read it:

```bash
cat /data/www/default/.hidden_file.txt
```

---

## Forensics — Finding the Miner

Check running services:

```bash
systemctl list-units --type=service --state=running
ps aux
```

There's a suspicious process: **`nm-inet-dialog`** running under a service called **`ubuntu.service`**.

Check the miner's log:

```bash
ls /lib/NetworkManager/
cat /lib/NetworkManager/inet.conf
```

The log file is **`inet.conf`**. It contains an encoded string — run it through CyberChef (Magic / Hex decode) to reveal the wallet address:

```
bc1qyk79fcp9hd5kreprce89tkh4wrtl8avt4l67qa
```

---

## Threat Intel — Who Owns That Wallet?

Look up `bc1qyk79fcp9hd5kreprce89tkh4wrtl8avt4l67qa` on a blockchain explorer (Blockchair, blockchain.com). The transaction trail links it to **LockBit** ransomware operations.

---

## 🏁 Questions & Answers

| Question | Answer |
|---|---|
| Content of the hidden .txt file in the web folder | `THM{fl46_650c844110baced87e1606453b93f22a}` |
| What is the suspicious process name? | `nm-inet-dialog` |
| What is the service name affiliated with the suspicious process? | `ubuntu.service` |
| What is the log file name of the miner instance? | `inet.conf` |
| What is the wallet address of the miner instance? | `bc1qyk79fcp9hd5kreprce89tkh4wrtl8avt4l67qa` |
| Which threat group is associated with the wallet? | `LockBit` |

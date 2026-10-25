# Snort: TryHackMe Walkthrough

Official room: https://tryhackme.com/room/snort

## Room Overview & Information

**Category:** Network detection and prevention  
**Difficulty:** Beginner to intermediate  
**Focus:** IDS/IPS concepts, Snort rules, sniffing traffic, analysing PCAP files, and validating rule matches.

Snort is a rule-based network intrusion detection and prevention system. In IDS mode it identifies matching traffic and generates alerts. In IPS mode, when configured inline with appropriate actions, it can drop or reject traffic. This room provides a VM with task-specific exercise folders and sample packet captures. Use those files rather than substituting traffic from a different lab.

## Task 1: Introduction

IDS means Intrusion Detection System; IPS means Intrusion Prevention System. A detection system observes and alerts, while a prevention system can actively block traffic depending on its deployment and rule action.

A Snort rule generally includes an action, protocol, source address and port, direction operator, destination address and port, plus a rule body containing options such as message, content, flags, or packet fields. Rule accuracy depends on correct syntax and the quality of the traffic being inspected.

## Task 2: Interactive material and VM

Start the room's machine and open the split-screen view. The desktop contains `Task-Exercises` with sample configuration and rule files under `Config-Sample`, and per-task traffic and PCAP files under `Exercise-Files`.

The samples are intended for practice. The installed Snort configuration is normally under `/etc/snort`. Back up any configuration before editing it, and do not overwrite a working rule when a commented example would be enough.

## Task 3: Snort configuration and rules

The room demonstrates the difference between the sample configuration and the installed configuration. A line beginning with `#` is commented out and does not take effect. Rule includes must point to the intended rule file. When a rule does not fire, check the rule syntax, include path, action, addresses, ports, protocol, and whether the traffic actually matches the condition.

## Task 4: IDS/IPS rule behaviour

An alert rule reports matching traffic. A drop or reject action can prevent the traffic when Snort is operating in a mode and network position that supports enforcement. Merely changing a rule's action does not make Snort an inline IPS; deployment mode matters.

Always test rules on the provided exercise traffic first. A broad rule can generate too many alerts or block legitimate traffic.

## Task 5: Operation Mode 1: Sniffer Mode

Sniffer mode prints packets as Snort observes them, similar to tcpdump. The options used in the room include:

```bash
sudo snort -v -i eth0
```

- `sudo` runs Snort with the privileges required to capture packets.
- `snort` launches the application.
- `-v` displays verbose TCP/IP information.
- `-i eth0` selects the network interface named `eth0`.

To see packet payload and link-layer headers:

```bash
sudo snort -dev -i eth0
```

The combined flags include verbose output, payload data, and link-layer headers. The interface must exist on the VM. Snort needs traffic to inspect, so the room supplies a traffic-generator script.

## Task 6: Operation Mode 2: Packet Logger

The room asks you to investigate traffic using ASCII logging:

```bash
cd ~/Desktop/Task-Exercises/Exercise-Files/TASK-6
sudo snort -dev -K ASCII -l .
sudo ./traffic-generator.sh
```

Use the actual folder path shown on the VM if the desktop path differs. Choose **TASK-6 Exercise** in the generator, wait for the traffic to finish, then stop Snort with Ctrl+C.

- `-d` displays packet payload data.
- `-e` displays link-layer headers.
- `-v` enables verbose output.
- `-K ASCII` selects ASCII packet logging.
- `-l .` writes logs to the current directory.
- The traffic generator creates the exercise traffic.

The output includes a directory named after an address. Navigate into the directory the task identifies and inspect the packet data.

### Question 1: “What is the source port used to connect port 53?”

Open the generated log directory named `145.254.160.237` as instructed. Identify the packet whose destination service port is 53 (DNS), then read the source port from the same packet. Do not mistake destination port 53 for the source port. The exact answer must come from the VM's captured traffic.

### Question 2: “Read the snort.log file with Snort; what is the IP ID of the 10th packet?”

Use the supplied log file:

```bash
snort -r snort.log.1640048004 -n 10
```

- `-r` reads an existing Snort log/PCAP-format capture.
- `-n 10` limits processing to ten packets.
- Inspect packet number 10 and read its IPv4 Identification field (IP ID).

The IP ID is a packet header field; it is not the packet's sequence number in the file.

### Question 3: “Read the ‘snort.log.1640048004’ file with Snort; what is the referer of the 4th packet?”

Read the capture and inspect packet 4's HTTP headers. The Referer header, when present, indicates the page that led the client to request the current resource. If the field is absent, verify that you are examining the correct packet and the correct task file.

### Question 4: “Read the ‘snort.log.1640048004’ file with Snort; what is the Ack number of the 8th packet?”

Inspect packet 8 and read its TCP acknowledgement number. Ensure the packet is TCP and use the acknowledgement field from that packet, not the sequence number or a neighbouring packet.

### Question 5: “Read the ‘snort.log.1640048004’ file with Snort; what is the number of the ‘TCP port 80’ packets?”

Count the packets matching TCP port 80 in the capture summary or use Snort's output to inspect the relevant traffic. Be clear whether the question means source or destination port 80; use the task's stated interpretation and verify the count against the capture.

## Task 7: Operation Mode 3: IDS/IPS

The room demonstrates running Snort with the default configuration and full alert output:

```bash
sudo snort -c /etc/snort/snort.conf -A full -l .
sudo ./traffic-generator.sh
```

- `-c /etc/snort/snort.conf` loads the configured rules and settings.
- `-A full` prints detailed alert information.
- `-l .` writes logs to the current directory.

Choose **TASK-7 Exercise** in the traffic generator. Wait for the test traffic to finish and stop Snort.

### Question: “What is the number of the detected HTTP GET methods?”

Inspect the generated alert/log output and count the events that match the rule for HTTP GET traffic. Use the alert output from the current VM rather than guessing from the traffic-generator name.

## Task 8: Operation Mode 4: PCAP Investigation

Snort can inspect a saved packet capture without listening to a live interface. This is useful for incident response because the analyst can replay evidence repeatedly while testing a rule.

A common pattern is:

```bash
snort -c local.rules -A full -l . -r task9.pcap
```

- `-c local.rules` loads the local rule file.
- `-A full` enables full alert output.
- `-l .` writes logs in the current directory.
- `-r task9.pcap` reads the supplied packet capture.

Before rerunning, clear or archive the prior alert output so you do not confuse old alerts with the current rule's results.

### Question 1: “Use ‘task9.pcap’. Write a rule to filter IP ID ‘35369’ and run it against the given pcap file. What is the request name of the detected packet?”

1. Open the TASK-9 exercise directory.
2. Edit `local.rules` and add a rule matching the specified IP ID.
3. Run Snort against `task9.pcap` with the command above.
4. Inspect the alert and the matching packet payload.
5. Identify the HTTP request name/method shown by the packet.

The exact rule syntax must match the installed Snort version. Verify the rule loads without a syntax error before interpreting the absence of an alert.

### Question 2: “Clear the previous alert file and comment out the old rules. Create a rule to filter packets with Syn flag and run it against the given pcap file. What is the number of detected packets?”

Disable previous rules by commenting them out, clear or archive the old alert file, create the SYN-flag rule, rerun the PCAP, and count the newly generated alerts. SYN is the TCP flag used during connection establishment; do not count packets merely because they use TCP.

### Question 3: “Clear the previous alert file and comment out the old rules. Write a rule to filter packets with Push-Ack flags and run it against the given pcap file. What is the number of detected packets?”

Repeat the clean test procedure with a rule matching both PSH and ACK flags. Ensure the rule is matching the intended flag combination, not either flag independently. Count only the alerts from the new run.

### Question 4: “Clear the previous alert file and comment out the old rules. Create a rule to filter UDP packets with the same source and destination IP and run it against the given pcap file. What is the number of packets that show the same source and destination address?”

Write a rule that matches UDP packets whose source and destination IP addresses are the same, then replay the capture. Count the resulting matched packets. If the rule produces no output, check address variables, direction, protocol, and rule syntax.

## Rule maintenance and final question

The room also presents a configuration case in which an analyst has modified an existing rule and asks which rule option must be changed after implementation. Inspect the provided configuration and the include lines. A line prefixed by `#` is commented out; uncommenting the correct include enables the intended rules file. Follow the sample's exact context to identify which include path must be active.

## Reporting and remediation

Back up configuration files before edits. Test new rules against the supplied PCAP and known benign traffic, make one change at a time, and keep old working rules available for rollback. Use clear messages and appropriate severity. In production, validate rule performance and false-positive rates before enabling packet drops.

## Key takeaways

Snort has distinct sniffing, logging, alerting, and PCAP-analysis workflows. Flags control what traffic is displayed or recorded, rules determine what matches, and deployment mode determines whether traffic can be blocked. The task-specific answers must be read from the provided captures and generated logs; do not invent packet fields or counts.

## Task and answer verification checklist

| Task focus | Correct evidence source |
|---|---|
| Source port when destination port is 53 | Packet in the generated ASCII log; read both source and destination ports |
| IP ID of packet 10 | Packet 10 from the supplied Snort log or PCAP |
| Referer of packet 4 | HTTP Referer header, if present, in the specified packet |
| ACK number of packet 8 | TCP acknowledgement field of packet 8 |
| Number of TCP port 80 packets | Count the specific traffic requested by the room, confirming whether source or destination port is meant |
| Number of detected HTTP GET methods | Alerts generated by the TASK-7 traffic and its rule |
| Request name/method matching IP ID 35369 | Alert and packet payload from TASK-9 PCAP |
| SYN / PSH-ACK detection count | Count alerts after disabling prior rules and clearing or archiving old output |
| UDP packets with same source/destination IP | Count packets matched by the rule in the supplied PCAP |

Machine-specific packet values and counts must come from the current VM. Don't copy a count or field from another instance, and don't confuse a source port with destination port or an ACK value with the TCP sequence number.

## Final operational checklist

Back up configuration before editing, verify the rule file loads successfully, run each test against the correct PCAP or generated traffic, and clear/rename old alert output so results are not mixed. Evaluate both true positives and false positives before enabling blocking actions. Snort's action depends on its mode and network placement; a local alert configuration by itself does not guarantee that traffic is being dropped.

# AI Forensics

{% embed url="https://tryhackme.com/room/aiforensics" %}

Leveraging Machine Learning and AI models within Digital Forensics and Incident Response (DFIR): anomaly detection algorithms, sentiment analysis, forensic admissibility under the Daubert standard, and practical breach investigation at RobbCo.

---

## 1. AI in the Forensics Pipeline

- **Anomaly Detection:** Unsupervised and semi-supervised models spot statistical deviations across millions of logs that manual review cannot scale to inspect.
- **Precision:** The proportion of flagged security events that are true positives.
- **Computer Vision (CNNs):** Convolutional Neural Networks automate optical forensic identification across images and video.
- **Legal Admissibility (Daubert Standard):** In legal jurisdictions, the Daubert standard governs whether expert testimony based on scientific methods or AI algorithmic outputs is admissible in court.

---

## 2. Practical Investigation at RobbCo

Activating the lab virtual environment:
```bash
source /opt/dfir-env/bin/activate
python3 /opt/dfir-lab/classify_logs.py /var/log/auth.log
```

Forensic findings from the digital trail:
1. **Initial Compromise:** Spear **Phishing** email from `akeane@poseidonenergy.net`.
2. **Initial Foothold Login:** Compromised user `j.morgan` authenticated at **`03:01:02`**.
3. **Lateral Movement:** The adversary injected their public SSH key into the `r.house` profile via:
   `sudo nano /home/r.house/.ssh/authorized_keys`
4. **Data Staging & Exfiltration:** The staged encrypted archive was hidden in shared memory:
   `/dev/shm/.core_dump_2025.tgz.enc`

---

## 🏁 Questions & Answers

| Task | Question | Answer |
|---|---|---|
| **Task 2** | AI ability helping DFIR recognize complex patterns | `Anomaly Detection` |
| **Task 2** | Metric: proportion of positively flagged results that were correct | `Precision` |
| **Task 3** | Neural network used in visual and video forensics | `Convolutional Neural Network` |
| **Task 4** | U.S. legal standard assessing admissibility of scientific testimony | `Daubert standard` |
| **Task 5** | Time attacker logged in as `j.morgan` | `03:01:02` |
| **Task 5** | Initial access method | `Phishing` |
| **Task 5** | Attacker's email address | `akeane@poseidonenergy.net` |
| **Task 5** | Command used to reach `r.house` | `sudo nano /home/r.house/.ssh/authorized_keys` |
| **Task 5** | Full path of the exfiltration archive | `/dev/shm/.core_dump_2025.tgz.enc` |

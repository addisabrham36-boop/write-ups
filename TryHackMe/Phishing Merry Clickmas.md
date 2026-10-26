# Phishing: Merry Clickmas

{% embed url="https://tryhackme.com/room/phishingmerryclickmas" %}

An Advent of Cyber red-teaming lab demonstrating social engineering simulation, credential harvesting via cloned login portals, and lateral account pivoting across enterprise webmail.

---

## 1. Attack Lifecycle & Scenario

- Setting up an external phishing landing page targeting the Toy Building Factory Corp (TBFC).
- Enticing employee victims into entering administrative credentials through targeted social engineering.

---

## 2. Exploitation Walkthrough

1. Harvest credentials submitted to the cloned portal:
   - Discovered password for the TBFC portal:
     `unranked-wisdom-anthem`
2. Test the harvested password against the company webmail interface (`http://MACHINE_IP/mail`):
   - Access the factory inbox.
   - Read the confidential delivery logistics email detailing seasonal shipments.
   - The verified toy count expected for delivery is:
     `1984000`

---

## 🏁 Questions & Answers

| Task | Question | Answer |
|---|---|---|
| **Task 1** | Password used to access the TBFC portal | `unranked-wisdom-anthem` |
| **Task 2** | Total number of toys expected for delivery | `1984000` |

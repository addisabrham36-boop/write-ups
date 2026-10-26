# Windows Basics

{% embed url="https://tryhackme.com/room/windowsbasics" %}

A practical introduction to the Windows operating system for security beginners: navigating the GUI, inspecting device and OS specifications, reviewing Task Manager metrics, and running malware scans with Windows Security.

---

## 1. System Specifications

To view system hardware and OS information:
1. Open the **Start Menu**.
2. Search for and open **About your PC**.
3. Under **Device specifications**, check the installed RAM (typically **4 GB** in the THM lab VM).
4. Under **Windows specifications**, verify the Windows Server version build (**1809**).

---

## 2. Desktop & File Exploration

Navigating to the **TryHatMe Onboarding** folder on the Desktop:
- Inside `Welcome.txt`, open the document to find the introductory welcome flag.
- Executing the `TryHatMeWelcome` installer triggers the setup routine and generates the application completion flag.

---

## 3. Task Manager & Performance

1. Press `Ctrl + Shift + Esc` to open **Task Manager**.
2. Click the **Performance** tab and select **CPU**.
3. The real-time CPU base speed is displayed in GHz (e.g., `2.40 GHz`).

---

## 4. Antivirus & Threat Protection

Testing the built-in antivirus (Windows Defender) using an EICAR standard anti-malware test string:
1. Launch **Windows Security** from the desktop shortcut.
2. Navigate to **Virus & threat protection** -> **Scan options**.
3. Select **Custom scan** and choose the `TryHatMe Onboarding` folder.
4. Windows Defender identifies `Tool:Win32/EICAR_Test_File`.
5. Clicking **See details** reveals the detected item filename: **`EICAR.txt`**.

---

## 🏁 Questions & Answers

| Task | Question | Answer |
|---|---|---|
| **Task 2** | Installed RAM in Device specifications | `4 GB` |
| **Task 2** | Version of Windows Server 2019 Datacenter installed | `1809` |
| **Task 2** | Flag found in `Welcome.txt` | Dynamically generated instance flag (`THM{...}`) |
| **Task 3** | Flag received after running `TryHatMeWelcome` installer | Dynamically generated instance flag (`THM{...}`) |
| **Task 3** | Country or region in Time & Language settings | `United States` |
| **Task 3** | File name shown in Windows Security affected items | `EICAR.txt` |

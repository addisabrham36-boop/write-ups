# Windows Basics: TryHackMe Walkthrough

Official room: https://tryhackme.com/room/windowsbasics

## Room Overview & Information

**Category:** Operating-system fundamentals  
**Difficulty:** Easy  
**Focus:** Windows workspace, system specifications, configuration, Defender, and basic endpoint security.

The room places the learner in a simulated Windows Server 2019 Datacenter workstation. The tasks use the Windows Settings app, an onboarding folder, Task Manager, and Microsoft Defender. Values such as device name and region should be read from the active virtual machine.

## Task 1: Introduction

Read the learning objectives and start the provided Windows machine.

**Question: “I understand the learning objectives and am ready to learn about Windows!”**

Answer: **No answer needed.**

## Task 2: Exploring the Windows Workspace

### Question: “After opening About your PC, navigate to the Device specifications section. What is the Device name specified?”

1. Open the Start menu.
2. Open **Settings**.
3. Navigate to **System → About** or open **About your PC**, depending on the interface.
4. Find **Device specifications**.
5. Read the **Device name** field.

**Room answer:** `TryHatMe`.

The device name identifies the workstation on the network. In an enterprise, hostnames are useful for correlating endpoint alerts and inventory records, but they should not be treated as a secret or as proof of a device's identity.

### Question: “Continue looking through the Device specifications. How much RAM is installed on your new work PC?”

**Room answer:** **4.00 GB**.

Read the installed RAM field in the same Device specifications section.

### Question: “Scroll down to the Windows specifications section. Which Version of Windows Server 2019 Datacenter is installed?”

**Room answer:** **1809**.

The version is displayed under **Windows specifications**. Record the version shown by the virtual machine rather than relying on the room's title alone.

### Question: “Explore the TryHatMe Onboarding folder located on your computer's Desktop. What is the flag value found within Welcome.txt?”

1. Open File Explorer.
2. Navigate to the Desktop.
3. Open the **TryHatMe Onboarding** folder.
4. Open **Welcome.txt** in a text editor.
5. Submit the flag shown in the file.

The flag is redacted here as `THM{hidden}`.

## Task 3: Configuring and Securing Windows

### Question: “Use the TryHatMeWelcome installer located within the TryHatMe Onboarding folder. What is the flag value you receive after installing and running the application?”

1. Open the **TryHatMe Onboarding** folder.
2. Run the **TryHatMeWelcome** installer supplied by the room.
3. Follow the installer prompts, confirm the installation, and launch the application.
4. Read the message displayed by the installed application.
5. Submit the flag shown by the active machine.

The flag is redacted as `THM{hidden}`. Install only the software supplied by the authorised room.

### Question: “Investigate the Time & Language section of the Windows Settings app. Which country or region is your computer currently set to?”

1. Open **Settings**.
2. Select **Time & Language**.
3. Open **Language & Region** or the equivalent region settings page.
4. Read the **Country or region** value.

**Room answer:** **United States**.

Regional settings affect date, time, language, and formatting. They do not by themselves establish the physical location of a computer.

### Question: “Open the Task Manager on your workstation's Desktop and navigate to the Users tab. Which account is currently logged in?”

1. Open Task Manager using the Desktop shortcut or Ctrl+Shift+Esc.
2. Select the **Users** tab.
3. Identify the active logged-in account.

**Room answer:** **Administrator**.

A Windows account name does not prove that every process is running with elevated privileges. For security work, inspect the process token and group membership when privilege level matters.

### Question: “After performing your custom scan, click Virus:DOS/EICAR_Test_File and select See details. What is the file name shown in the Affected items section?”

1. Open Microsoft Defender or Windows Security as the room instructs.
2. Run the custom scan from the provided onboarding folder.
3. Open the detection named **Virus:DOS/EICAR_Test_File**.
4. Select **See details**.
5. Read the **Affected items** section and copy the filename.

**Room answer:** `tryhatmemaldoc.txt`.

The EICAR test file is a harmless standard test string used to verify antivirus detection. It is not real malware. Windows may quarantine the test file before the scan starts; if so, open **Threat history**, locate the quarantined item, and inspect its details as the room instructs.

## Task 4: Conclusion

Complete the tasks and submit any flags shown by the active room. No additional technical answer is required for the conclusion task.

## Security lessons

- Keep Windows and applications updated.
- Use least privilege and avoid using an administrator account for routine work.
- Enable endpoint protection and review detections rather than assuming a scan is complete.
- Verify the source of installers before running them.
- Monitor endpoint events and preserve relevant logs.
- Treat hostnames and system specifications as inventory data, not authentication factors.

## Key takeaways

The room teaches practical navigation of Windows Settings, the filesystem, Task Manager, and Microsoft Defender. The exact flags are obtained from the onboarding file and application in the live VM; they are redacted in this public write-up.
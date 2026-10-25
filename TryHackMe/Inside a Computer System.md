# Inside a Computer System — TryHackMe

Room: https://tryhackme.com/room/insideacomputersystem

A computer system is made of hardware and software working together. The CPU executes instructions, RAM holds data and instructions currently in use, and storage keeps data when power is off. The operating system coordinates these resources and gives applications a consistent way to use them.

## The main components

The CPU processes instructions. RAM is fast working memory, but it is volatile, so its contents are not normally retained after power is removed. SSDs and hard drives provide persistent storage. Input and output devices let the system communicate with users and other equipment.

The motherboard connects components, while firmware helps initialise hardware and start the boot process. The operating system then loads and manages applications, memory, files, and devices.

## Why security cares

Different components leave different kinds of evidence. Storage may contain files and logs, RAM may contain data that disappears after shutdown, and network interfaces reveal communication with other systems. This is why forensic investigators consider the order of collection: powering off a machine can destroy volatile evidence.

## What I took from this room

Understanding the basic hardware and software layers helps me understand where data lives and what evidence may be available. It also explains why security investigations need to consider both persistent storage and volatile system state.
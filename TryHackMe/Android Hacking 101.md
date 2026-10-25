# Android Hacking 101 — TryHackMe

Room: https://tryhackme.com/room/androidhacking101

Android security testing involves understanding application packages, permissions, components, storage, and network communication. The work should be done on an emulator or device that I own or have permission to test, ideally with a deliberately vulnerable training app.

## APK basics

An APK is an Android application package. Static inspection can reveal the manifest, declared permissions, components, resources, and code. The manifest helps explain what the application asks Android to allow, but a declared permission does not automatically prove that the app misuses it.

Tools such as JADX can help inspect decompiled code, while Android platform tools can interact with an emulator. Decompiled output is not always identical to the original source, so I should treat it as an aid to understanding rather than a perfect reconstruction.

## Dynamic testing

Dynamic analysis observes an app while it runs. I can inspect network requests, logs, files created by the app, and how it behaves when given different inputs. A test environment should use dummy accounts and data, not personal credentials.

## What I took from this room

Android testing combines static and dynamic analysis. I need to understand the app's permissions and components, observe its behaviour, and test only within an authorised lab. A suspicious permission or string is a clue to investigate, not proof of a vulnerability by itself.
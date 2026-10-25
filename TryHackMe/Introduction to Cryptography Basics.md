# Cryptography Basics — TryHackMe

Room: https://tryhackme.com/room/cryptographybasics

Cryptography provides techniques for protecting information and verifying that it has not been changed. The main ideas I need to distinguish are confidentiality, integrity, authentication, and non-repudiation.

## Encryption and keys

Symmetric encryption uses one shared secret key for encryption and decryption. Asymmetric cryptography uses a related public and private key pair. These approaches can be combined in secure protocols so that asymmetric methods help establish trust or exchange key material while symmetric encryption protects the main data stream.

The security of encryption depends on correct implementation and key handling, not just choosing an algorithm with a strong reputation.

## Hashing and signatures

A hash function produces a fixed-size digest from input. It is useful for integrity checks, but it is not intended to be decrypted. Digital signatures use cryptography to help verify that data was signed by the holder of a private key and has not changed since signing.

## What I took from this room

Cryptography is a set of tools with different purposes. I should identify whether a problem needs confidentiality, integrity, or authenticity before choosing a method. Encoding, encryption, hashing, and signatures are not interchangeable, even when their outputs all look like strange strings.
---
name: ctf
description: CTF challenge playbook (route R8). Use for CTF/攻防/靶场 challenges: quick-win triage per category (web, crypto, forensics, reverse, pwn, misc), flag capture, writeups. Routes to the deeper module when a challenge grows beyond quick wins (APK→R1, binary→R3, pwn→R6). CTF platforms carry their own authorization.
---

# CTF playbook (R8)

## When to use
CTF challenges, 靶场, AWD/攻防, writeups, flag capture. The CTF platform's
rules are the authorization basis (scope preset: `ctf-public`).

## Workflow (ACTION REQUIRED)
1. **Scope**: `case-init --preset ctf-public --target-url <challenge>`. Record
   challenge + flag submission as evidence.
2. **Triage by category** — 5 minutes per category, then commit to one:

| Category | Quick wins |
|----------|-----------|
| Web | robots.txt, source comments, admin creds, IDOR, SQLi basics, JWT alg=none |
| Crypto | base64/hex/ROT/XXTEA, XOR with known plaintext, RSA small e/n, AES key reuse |
| Forensics | strings, binwalk, file signatures, stego (zsteg), PCAP follow streams |
| Reverse | strings, `file`, simple checks, angr for brute-forceable checks (R3) |
| Pwn | checksec, obvious overflow → R6 chain |
| Misc | OSINT, archive tricks, LSB stego, QR, magic bytes |

3. **Chain up**: when a challenge exceeds quick wins, hand to the owning
   module (R1/R3/R6/…) — same case dir, keep the evidence chain.
4. **Writeup**: flag + solve path + E-* chain. Writeups are the deliverable
   (and feed the field-journal).

## Boundaries
- The platform is the authorization. Never attack the CTF platform itself or
  other teams' infrastructure (in AWD, stay within the rules of the round).

## Evidence
- Every step: command + output; flag submission timestamp as final evidence.

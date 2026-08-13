---
name: pwn-chain
description: Exploit development chain (route R6). Use for CTF/lab pwn challenges and authorized exploit development: stack overflow, heap exploitation, ROP, ret2libc, format string, kernel pwn basics, with pwntools + gdb. Note: pwn tooling works best in a Linux VM — the macOS host is for writing/scripting, not reliable exploitation.
---

# Pwn / exploit development (R6)

## When to use
CTF pwn, 栈溢出/堆溢出 challenges, ROP chains, ret2libc, UAF, format string,
shellcode, kernel pwn (lab VM). Authorized/lab targets only.

## Tools
- Python + pwntools: `pipx install pwntools` (host)
- gdb: `brew install gdb` (host) — but the real work happens in a Linux VM:
  `sudo apt install gdb pwndbg gef` there
- checksec: `pwn checksec <binary>` (from pwntools)
- one_gadget / ropper: `pipx install ropper` (host)

## Workflow (ACTION REQUIRED)
1. **Scope**: ctf-public or own lab. Download the binary; sha256 → E-001.
2. **Recon**: `file`, `checksec` (PIE/NX/Canary/RELRO) — record as E-*.
3. **Understand the bug**: read the decompile (R3 tools) → identify the
   vulnerability class + constraint (offset, size, leak available?).
4. **Leak phase** (if needed): craft the leak input; extract libc/stack/PIE
   base; document addresses as evidence.
5. **Exploit**: write `solve.py` with pwntools; iterate in the Linux VM;
   local flag/crash first, then remote (in scope). Every iteration logged.
6. **Finish**: `solve.py` + a 10-line explanation of the chain = deliverable.
   Write the technique to field-journal (it's a reusable pattern).

## Boundaries
- Remote targets only if explicitly in scope (CTF platform = yes; anything
  else needs written auth).
- No persistence, no lateral movement off the challenge box.

## Evidence
- checksec output, addresses/offsets from the decompile, final solve.py run
  output with flag — all E-* entries feeding one Finding ("challenge solved",
  category: pwn).

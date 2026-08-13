# reverse-skill routing rules

Single source of truth for how an agent should handle security / reverse /
teardown tasks. Client-neutral: works from pi, Claude Code, Codex, Cursor, or
any agent that reads instruction files. Scripts are bash (macOS/Linux).

## Mode detection (read this FIRST)

1. Is the task about **software we did not write** (competitor repo, upstream
   package, any open-source project) and the goal is to **understand it so we
   can build our own version** → **Mode 2 teardown → REBUILD**
   (`skills/teardown/SKILL.md`). Read-only against the target; then BUILD our
   version in `/Users/bishesha/projects/<name>` — scaffold, implement the
   Paths, run checks, review round (loop skill), ship. Advice-only is opt-in.
2. Otherwise, is it **security RE / pentest / CTF** on an authorized target
   (own system, CTF platform, lab, written engagement) → **Mode 1**.
3. Neither → it is not a reverse-skill task. Do not force it.

## Routing entry (Mode 1)

```
1. bash skills/scripts/master-route.sh --hint "<task>"   → PRIMARY (reads config/routing.json)
   or read skills/MASTER-ROUTING.md directly
2. bash skills/scripts/case-init.sh --hint "<task>" --preset <offline-sample|ctf-public|own-system>
   → creates work/<case>/ with scope.md, timeline.md, workitems.md
3. READ work/<case>/scope.md. ACT against targets ONLY when:
   - auth.status = granted, AND
   - in_scope.assets set, AND
   - network_profile chosen
4. Open the PRIMARY module SKILL.md → execute its workflow.
5. Append timeline.md + workitems.md as you go; evidence per ops/evidence-finding-path.md.
6. Finish: docs-generator report + anonymized field-journal write-back.
```

## Tool usage (MUST)

- Read `skills/tool-index.md` (auto-generated, gitignored) before using any tool.
- **Never guess paths** — tool-index.md is the only source of tool locations.
- Missing tool → check the module SKILL.md for install hints (Homebrew preferred
  on macOS), install, then `bash skills/scripts/refresh-tool-index.sh`.
- If tool-index.md doesn't exist (first run): run refresh-tool-index.sh now.

## Gates (MUST NOT)

- MUST NOT scan / hook / exploit / modify any target without scope.md showing
  `auth.status=granted` + `network_profile` (see skills/ops/scope-contract.md).
- MUST NOT expand beyond in_scope assets.
- MUST NOT exfiltrate or retain real-user PII; anonymize reports and journals.
- MUST NOT do anything on this list without explicit owner confirmation:
  DoS, phishing real users, destructive actions on systems we don't own.

## Completion checklist (MUST)

```
□ 1. Report written (docs-generator or teardown proposal doc)
□ 2. Findings reference evidence (E-* or file:line)
□ 3. field-journal write-back (anonymized, if reusable lesson)
□ 4. Routing didn't match → propose a new route (don't force-fit)
```

## Anti-patterns

- ❌ "I already know how, I don't need the playbook" — read the module first.
- ❌ Guessing tool paths instead of reading tool-index.md.
- ❌ Starting on a target before scope.md is ready_for_act.
- ❌ Copying competitor code wholesale into our repo (MIT targets: adapt with
  attribution only; keep evidence).
- ❌ Acting on ambiguity instead of asking the owner which mode this is.

## Trigger keywords (bilingual, ANY match → Mode 1 routing)

APK, Android, 反编译, smali, jadx, apktool, Frida, hook, JS reverse, 前端签名,
加密参数, sourcemap, binary analysis, 逆向, 反汇编, disassembly, radare2, ghidra,
malware, 恶意软件, YARA, pentest, 渗透, nmap, nuclei, sqlmap, ffuf, CTF, pwn,
exploit, 漏洞利用, firmware, 固件, binwalk, certificate pinning, 证书校验,
抓包, packet capture, request replay, report, writeup, 报告
(teardown/competitor keywords → Mode 2 instead)

## Routing table (R0–R11) — see skills/MASTER-ROUTING.md

# Scope contract (hard gate before ACT)

**MUST**: no security/RE/teardown task touches its target (scan, hook, exploit,
modify, or modify nothing but *act* on it) until `work/<case>/scope.md` exists
with `auth.status=granted` and a `network_profile`. No scope → read docs and
route only. This gate exists because the package is designed to move fast —
the scope file is the one thing that slows it down safely.

## Initialize

```bash
# local sample (offline): safe preset
bash skills/scripts/case-init.sh --hint "apk reverse" --preset offline-sample --sample ./app.apk

# CTF platform challenge: ctf preset
bash skills/scripts/case-init.sh --hint "ctf web" --preset ctf-public --target-url https://chal.example

# your own lab/hosts
bash skills/scripts/case-init.sh --hint "pentest own lab" --preset own-system --target-url https://lab.local
```

Anything else (someone else's production system, a client, a bug bounty
program): create the case, then fill scope.md auth manually and get the owner's
confirmation. `ready_for_act` must be `true` before ACT.

## scope.md contract (keys are checked by case-review)

```markdown
## meta      case_id, created, primary_skill, primary_id, hint
## auth      status: granted|pending|denied; basis; evidence_of_auth
             MUST NOT proceed if status != granted
## in_scope  assets [], surfaces [], activities []
## out_of_scope assets [], activities [] (default: dos, phishing_real_users, unrestricted_exfil)
## network_profile mode: offline | lab_only | authorized_target_only | unrestricted_lab
## deliverables report, field_journal, diagrams, timeline
## signoff    ready_for_act: true|false + checklist
```

## Guard (optional, pre-ACT)

```bash
bash skills/scripts/case-guard.sh -CaseRoot work/<case>   # exit 2 if not ready
```

## Teardown mode (R11)

Same file, but: assets = repo paths/URLs, network_profile = `offline` unless
the task needs network, and activities are read-only analysis only.

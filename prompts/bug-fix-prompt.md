# Bug Fix Prompt

Diagnose and fix a bug methodically — root cause, not symptom.

---

```
Fix the following bug in the codebase.

BUG:
<what happens>

EXPECTED:
<what should happen>

REPRODUCTION:
<steps / affected route / component>

PROCESS:
1. Identify the ROOT CAUSE before changing anything — explain it.
2. Propose the minimal, correct fix consistent with the handbook rules.
3. Check for the same bug pattern elsewhere in the codebase.
4. Add/adjust a test that fails before the fix and passes after.
5. Confirm no regressions (types, lint, related states).

CONSTRAINTS:
- No band-aid patches or silent try/catch swallowing.
- Keep the change focused; don't refactor unrelated code.

OUTPUT:
- Root-cause explanation
- The diff/files changed (full paths)
- The test proving the fix
```

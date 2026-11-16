<!-- markdownlint-disable -->

# Hardening Report: ChartBoost--ruff-action/v1.0.0

> This file was generated automatically by the hardening agent.

**Policy SHA:** `374911c4be584df24e6da57af2d990570ef0f859`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **ChartBoost--ruff-action/v1.0.0** was hardened automatically. 2 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### script-injection (severity: high)

Rule (b) violation in the composite step's run: block. $GITHUB_ACTION_PATH is expanded without quotes on the lines `python $GITHUB_ACTION_PATH/action/main.py` and `python3 $GITHUB_ACTION_PATH/action/main.py`. This env var is read in a composite action, so the check says to treat it as untrusted and double-quote it. If left unquoted, the shell splits the value on whitespace and expands globs, which can break the command or run the wrong file. Fix: use `python "$GITHUB_ACTION_PATH/action/main.py"` and `python3 "$GITHUB_ACTION_PATH/action/main.py"`.

Locations:

- `action.yml:27`
- `action.yml:29`

### prior-fix (severity: high)

Carried forward from this version's previous harden, which fixed it; fix it again unless the code no longer has the problem. The previous harden changed action.yml without recording a finding for it, and this analysis reported nothing there. Re-apply its security changes unless the code no longer needs them. The previous patch:
```diff
diff --git a/action.yml b/action.yml
index fd98e29..931eb4d 100644
--- a/action.yml
+++ b/action.yml
@@ -3,9 +3,7 @@ description: "A GH Action of Ruff, the extremely fast Python linter."
 author: "Austin Bennett and the Ruff Community"
 inputs:
   args:
-    description:
-      "Arguments passed to Ruff. Use `ruff --help` to see available options. 
-      Default: 'check'"
+    description: "Arguments passed to Ruff. Use `ruff --help` to see available options. Default: 'check'"
     required: false
     default: "check"
   src:
@@ -22,6 +20,23 @@ branding:
 runs:
   using: composite
   steps:
     - run: |
         if [ "$RUNNER_OS" == "Windows" ]; then
           python $GITHUB_ACTION_PATH/action/main.py

```

Locations:

- `action.yml`

## Iteration Notes

### Iteration 1

**Fixes applied:** script-injection, prior-fix

**Notes:**

I put double quotes around $GITHUB_ACTION_PATH/action/main.py in both the python and python3 calls in action.yml. That fixes the script-injection finding. For the prior-fix finding, I redid the change I could see from the earlier patch: the args input description now sits on one line. The diff I was given cuts off after its first lines of the step, so its other 17 added lines are unknown and weren't redone. The step already passes inputs through env: and main.py splits them with shlex instead of a shell, so I didn't change anything else. Nothing was run or tested.


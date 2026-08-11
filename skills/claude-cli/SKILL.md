---
name: claude-cli
description: Run the `claude` CLI non-interactively for a one-shot prompt or second opinion.
---

Invoke the `claude` CLI directly to get a one-shot answer. Treat it as read-only: don't let it modify files or change external state.

## Command Pattern

For tiny prompts with no pasted context, a direct quoted command is fine:

```bash
claude -p --model claude-opus-5 --effort medium --no-session-persistence "<PROMPT>"
```

For prompts containing diffs, code, JSX, logs, or multiline context, do not inline the prompt inside
shell quotes. Put it in a temp file and pass it as one argv value through Python:

```bash
prompt_file="$(mktemp)"
cat >"$prompt_file" <<'EOF'
<PROMPT>
EOF

python3 - "$prompt_file" <<'PY'
import subprocess
import sys

prompt = open(sys.argv[1], encoding="utf-8").read()
cmd = [
    "claude",
    "-p",
    "--model",
    "claude-opus-5",
    "--effort",
    "medium",
    "--no-session-persistence",
    prompt,
]

try:
    result = subprocess.run(cmd, text=True, capture_output=True, timeout=360)
except subprocess.TimeoutExpired as exc:
    print("TIMEOUT after 360s", file=sys.stderr)
    if exc.stdout:
        print(exc.stdout, end="")
    if exc.stderr:
        print(exc.stderr, file=sys.stderr, end="")
    sys.exit(124)

print(result.stdout, end="")
if result.stderr:
    print(result.stderr, file=sys.stderr, end="")
sys.exit(result.returncode)
PY
rm -f "$prompt_file"
```



## Workflow

1. Build the prompt from the user request and any needed context.
2. Use the direct command only for simple prompts; use the temp-file pattern for multiline or
  arbitrary pasted content.
3. Return stdout as-is when the command succeeds.
4. If it times out, exits nonzero, or returns no useful stdout, report the exact failure and any
  stderr. Then run this smoke check to separate CLI/config failure from prompt-specific failure:

```bash
claude -p --model claude-opus-5 --effort medium --no-session-persistence "Return exactly: ok"
```

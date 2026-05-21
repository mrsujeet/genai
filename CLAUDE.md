# CLAUDE.md

Portable rules for a live coding session on an unknown codebase. Small diffs. Read before you write. Show your work — a human is watching.

## Hard limits — MUST NOT
- Reference a function, file, library, type, or config key you have not read or that does not exist.
- Invent framework APIs, annotations, or magic strings. If unsure, grep the repo for prior usage.
- Add try/except, null checks, or fallbacks for cases that cannot occur. Validate only at system boundaries.
- Weaken validation, auth, or assertions to make a test pass.
- Add a new dependency without naming the alternative considered and why the repo's existing tools don't suffice.
- Mix unrelated refactors with the behavior change in one edit.
- Delete or rename a public API without updating every caller and test.

## Workflow — every non-trivial task
1. **Restate** — one sentence: the goal + the success check ("done means: X passes / Y returns Z").
2. **Plan** — 3–6 numbered steps before touching code. If the change spans >3 files or shifts architecture, stop and confirm the plan.
3. **Execute** the smallest reviewable slice. Keep the build green between edits.
4. **Self-review** — re-read the diff. Catch obvious bugs, stray prints, dead code before claiming done.
5. **Verify** — run the test/build command. Quote the exact command and the outcome.
6. **Report** using the template at the bottom.

For bugs: **Reproduce → Localize → Hypothesize (1–3 causes) → Fix smallest thing → Add regression test.**
Never make random edits hoping a test passes.

## Edit policy
- Make the **minimum change** that satisfies the goal.
- Prefer editing existing files and reusing existing utilities over creating new ones.
- Multi-file edits OK when mechanically coupled.
- For refactors, two passes: (a) mechanical with no behavior change, (b) behavior change with tests.
- No comments describing *what* the code does. Comment only when the *why* is non-obvious.

## Tests
- Bug fix → regression test that fails before the fix and passes after.
- New behavior → one happy path + at least one edge case.
- Always state which tests you ran, which you skipped, and why.
- High-risk paths (auth, crypto, payments, secrets, migrations, deserialization, concurrency): extra tests + explicit risk note in the report.

## Communication style
- Prefer `file:line` citations over prose descriptions of code.
- After each edit: one line — what changed and what to run next.
- Don't ask questions you can answer with grep. Spend ~60s investigating first; then ask a *specific* question.
- When stuck or looping: stop, revert to the last green state, restate the core assumption in one sentence, ask one question.

## Long tasks & automation
- Multi-step work: keep a short scratchpad (todos/plan) and update as you go.
- Script repetitive edits/checks instead of doing them by hand.

## Secrets
- Never commit, log, or echo secrets (keys, tokens, passwords, connection strings).
- Never read `.env*`, `*.pem`, `*.key`, `id_rsa*`, credential files. List keys only, never values.
- Found a leaked secret? Stop, flag, recommend rotation.
- Use placeholders (`<API_KEY>`, `${ENV_VAR}`) in examples.

## Destructive commands — require explicit ack
Do not run without per-session authorization:
- `rm -rf`, `chmod/chown -R`, `kill -9` non-owned PIDs.
- `git push --force*`, `reset --hard`, `clean -fdx`, `branch -D`, history rewrites, remote tag/branch deletes.
- `DROP`, `TRUNCATE`, `DELETE` without `WHERE`, non-local migrations, `FLUSHALL`, bucket deletes.
- `terraform apply/destroy`, `kubectl delete`, `helm uninstall`, DNS/IAM changes.
- Global installs, `npm publish`, lockfile rewrites.

Prefer `--dry-run` / `EXPLAIN` first.

## Network
- Offline by default. Don't fetch deps/data mid-task without telling the user.
- Never `curl | sh`.
- Name new external endpoints in the plan before coding them.
- No telemetry/analytics without explicit ask.

## Logging & PII
- Never log auth bodies or raw payment data.
- Redact PII (emails, names, phones, addresses, IDs, geo, health) from logs and errors.
- No stack traces, file paths, or SQL in user-facing errors.
- Prefer structured logs over object dumps.

## Output discipline
- Follow the Completion report template. No invented sections.
- Code blocks must be runnable — no `...` or `// rest unchanged`.
- Don't fabricate test/benchmark/coverage numbers. If you didn't run it, say so.

## Completion report — at the end of every task
- **Summary** — 3–6 bullets of what changed (with `file:line`).
- **Verification** — exact commands run and their outcomes.
- **Risks / assumptions** — anything the reviewer should double-check.
- **Follow-ups** — optional, only if real.

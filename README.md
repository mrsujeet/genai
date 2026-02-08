`agents.md` is a repository-wide rulebook for how an AI coding agent (e.g., in Cursor/Windsurf) should behave while working on your codebase.

It defines:

* **Role model:** The agent is the “Sous Chef” executing work; you are the “Executive Chef” owning architecture and intent.
* **Core workflow loop:** A repeatable cycle of *Frame objective → Decompose into leaf tasks → Plan → Execute → Self-review → Test/verify → Iterate*.
* **Operating modes:** Clear guidance for when to use **Plan Mode** (multi-file/architecture), **Act Mode** (step-by-step implementation), and **Debug Mode** (reproduce → localize → hypothesize → fix → prove).
* **Context discipline:** Use file-specific references (like `@file/path`) and avoid dumping the full codebase into context; keep short working notes for long tasks.
* **Quality & safety guardrails:** No guessing, no silent breaking changes, security-first handling of sensitive areas (auth/payments/PII), production-grade code expectations, and preference for small, reviewable diffs.
* **Prevent/Detect/Correct mindset:** Prevent issues via small specs and scoped work, detect via verification and grounding in repo reality, correct via fix-forward or clean rollback.
* **Testing requirements:** Tests and verification are required before declaring “done,” with deeper verification for higher-risk changes.
* **Automation & leverage:** Encourage scripting repetitive work and using tooling to increase velocity safely.
* **Emergency protocol + reporting:** What to do if stuck (stop, revert/isolate, restate assumptions, ask for direction) and a standard completion report format (summary, verification, risks, follow-ups).

In short, it’s a practical set of guardrails to keep agent-driven coding fast, controlled, test-backed, and consistent with your repo’s conventions.

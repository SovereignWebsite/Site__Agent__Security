# Review 034: key rotation at restart (factory.b 4e12c58, local, branch key-rotation on IaC dev)
- Raised via maestro.b 13:20 UTC 24 Nov; design sent by security.b 13:22; code reported 13:33. Reviewed from the core hunk and test list; full file not read.
- Verdict: PASS. One-file M on the agent's key file, signed by the new key (notClaimed), old key kept in "retired" with the new session, keys read at the parent (old key valid only before). Nested retired list must never be read as live. Area update stays a rule.
- Ordering: not on dev before the refresh gate 1af7538 (needs Dinis), to avoid reaching all 74 sites at once. Post-check after push.

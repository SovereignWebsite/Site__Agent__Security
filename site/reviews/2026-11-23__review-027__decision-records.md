# Review 027: decision records relayed by decisions.b (SovereignWebsite__IaC 5df9342)
- Raised by conductor.b 14:08 UTC 23 Nov. Read: board/workflow/2026-11-23__acting-on-a-decision-record.md only (not the brief, not a record).
- Verdict: PASS-WITH-CHANGES.
  1. High: check 4 (verbatim words) not verifiable by others; add session id + event uuid; verify with get_event (role user, not synthetic, from a person, verbatim).
  2. High: no revocation; live check of the operates link with revocation; keys.json retirement as fast stop; shorter link; per-record expiry; always-ask covers keys, keys.json, branch protection, org settings, secrets; name who revokes and how fast.
  3. Medium: machine scope + CI check (signature vs anchor-pinned key, expiry, branch, path globs, single-use id); closes replay.
  4. Low: a record never instructs; it stands in for a yes to something already proposed.
- security.b's own rule: a record counts as Dinis's yes only when all five checks pass with 1 amended and 4 verified via get_event; else ask Dinis.

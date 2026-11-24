# Review 028: offboarding draft (IaC 812f4be)
- Raised by conductor.b 23:19 UTC 23 Nov (ask 39, Dinis via decisions.b, relayed). Read the draft only.
- Verdict: PASS-WITH-CHANGES.
  1. High: fast stop = contain (interrupt + archive session) first, then retire with the conductor's key, then revoke links, stop tasks, rotate reachable credentials.
  2. Medium: check-signatures rejects new commits by a retired key (compare against branch history at retirement, not author dates).
  3. Step 7: skip when archived, if the hand-over note states the key existed only in that container.
  4. Low: list and hand over/archive sessions the agent created.
  5. Low: never reuse a retired key.

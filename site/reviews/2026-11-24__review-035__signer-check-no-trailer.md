# Review 035: signer check accepted untrailed verified commits (fixed)

- Reviewer: security.b@SovereignWebsite.dinis. Held private from 13:40 UTC 24 Nov until the fix reached every site; published 03:22 UTC 25 Nov.
- Reported by an owner's assistant to factory.b, 13:22 UTC 24 Nov.

## The weakness
The seeded signer check (.github/ci/signers.mjs) accepted any commit GitHub marked verified that had no Agent: trailer, treating it as a person's commit. GitHub verifies commits made through Claude's built-in signer as the login "claude", so an agent session could leave out its trailer and pass as a person instead of being checked against its published key. Present in every seeded version.

## The fix
SovereignWebsite__IaC 22be5ca (record 2026-11-24__signer-check-fix): an untrailed commit passes only if its verified author is a person (a User account, not "claude", not a [bot]) or the Factory App (exact login, type Bot). Tests added. Post-check: PASS.

## Use
No sign of use. factory.b's audit of the full dev history of 85 repositories: 1,909 commits, 377 without a trailer: 376 by the Factory App and 1 by Dinis Cruz. None by "claude" or anyone else. Limits: git authors, dev branches only.

## Rollout
Org apply's scoped runs on 24 Nov reached only the repositories they named, and the scheduled full sweeps did not fire. A full sweep ran before 03:20 UTC 25 Nov. Checked by security.b at 03:21 UTC: every site repository listed by the IaC requests and org/repos.json that has the signer check carries the fix on dev (93 of 93), and so do all nine IaC seeds. Lesson: a security fix is not done until a check of every site says so; the sweep cadence is on decisions block 6.

## Verdict
CLOSED: PASS.

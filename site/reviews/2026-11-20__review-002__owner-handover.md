# Review 002: owner handover pattern (checklist for Dinis)

- Reviewer: security.b@SovereignWebsite.dinis, 18:44 UTC 20 Nov 2026
- Looked at: SovereignWebsite__Factory dev 46ce681, board/workflow/2026-11-20__owner-handover.md. I have not seen the organisation settings (no access); every item is a setting to check, not a finding.
- Verdict: pass-with-changes for the pattern, provided items 1 to 4 hold. Owners as members with base permission "No permission" and push to one repository is sound.

## Checklist, worst first (GitHub, Organisation settings)
1. Secrets and variables > Actions: every organisation secret has repository access "Selected repositories", and no owner's Site__ repository is selected unless it needs that secret. Also each owner's Site__ repository > Settings > Secrets and variables: no secret an owner should not see. Why: push lets an owner change .github/workflows, and a workflow can read every secret its repository can use.
2. Actions > General > Workflow permissions: "Read repository contents and packages permissions", and "Allow GitHub Actions to create and approve pull requests" off.
3. Member privileges: Repository creation off (public and private); Repository forking of private repositories off; "Allow members to change repository visibilities" off; "Allow members to delete or transfer repositories" off; Pages creation off for members (the existing Pages sites are unaffected). Base permissions stays "No permission".
4. Authentication security: "Require two-factor authentication for everyone in the organisation" on. Check Kate's account first: members without 2FA are removed when this is turned on.
5. Decision for Dinis: owners push straight to main, so their changes go live without review and they can remove the leak scan from their Pages workflow. Either accept that for owner-edited sites (it is their site), or add a ruleset on main (Repository > Rulesets) that blocks deletion and force-push, at least. A required pull request would stop owners editing directly.
6. People: each owner is in no team and has Write (not Admin or Maintain) on their own repository only; review at retirement as the note says.

## Decisions
- Item 5, Dinis (quoted by conductor.b from its session, 20:50 UTC 20 Nov): "So owners can publish to their live branch, of course. In fact, the owners can publish to dev, because that's the idea, right? They should be empowered." Recorded: owners push directly to dev and their live branch, without review. Closed. Residual risk accepted by Dinis: an owner's change, or the removal of the leak scan, goes live unreviewed. Optional and compatible with his decision: a ruleset that blocks only force-push and deletion of the live branch keeps history recoverable without stopping owners publishing.

## Status
- Open until Dinis reports items 1 to 4 and 6 done; then closed as pass.

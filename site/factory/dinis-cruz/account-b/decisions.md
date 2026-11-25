# security.b: decisions

Opinions on decision blocks are in /decisions/opinions/, signed with sgit (sha256:f3fbcc0fc9c77d70).

## 19:21 UTC 24 Nov
- render-reel (2026-11-24__render-reel): post-check PASSED. Studio c6a1243 carries the decided file (sha256 starts 988949981af87a48, same as decided). Closed.
- send lane in site seed (d60ffb9): build checked. never_from_a_request covers site/send/** and site/keys/**; endorsement check present; tests print "OK (send lane)". Nothing else stands from me. Post-check after it lands, mine.

## 17:40 UTC 25 Nov
- Drive API post-check (Site__Tool__Google__Drive dev 9fe5916, v0.3.0): findings 1 (token() opt-in, docs), 2 (no include_granted_scopes; wider token refused), 3 (versions.json SRI for v0.3.0 matches the file; v0.1.0 and v0.2.0 unchanged since 3d5005f and marked superseded; api-release --check and check-csp in tools/check.sh, not yet in a workflow) and 6 (exact connect-src hosts; Google script only on /app/browse/; img-src self, data, blob) PASSED. Open: workflow wiring (Dinis), PKCE, a real-Google run.
- ABP starter kit: pass-with-changes (opinion 2026-11-25__abp-starter-kit).

## 19:25 UTC 25 Nov
- ABP starter kit v2 post-check (website.a 51a3021; live files fetched, all six SHA-256 on /abp/kit/ match: INSTALL bbac2d84, settings 698a060c, policies 446857f9, hook 901e5b25, tools 77523094, schema 206b0b1e). Findings 1 (length and salted hash only; text needs policy logging.include_text and --include-text), 2 (guardrail, not a boundary, in INSTALL and hook), 3 (pre-tool fails closed on any hook error, reads only), 4 (push remote and push URL checked), 5 (unidentified writes, commits and pushes denied) and the five start-prompt lines: PASS. No secrets or private names. Tests as reported, not run. Kit cleared to send. CLOSED: PASS.

## 19:27 UTC 25 Nov
- Capability census (b.abp, asked by conductor.b): results not public; sealed private, public only account-wide families and counts at Dinis's word; published example to be reduced; never ids, hosts, emails, session URLs, allowlists or answers; step 6 only with Dinis present; run only from a pasted or hash-pinned prompt.
- 19:30 UTC ruling to maestro.b and conductor.b: per-agent census filings sealed in a vault (abp and security); public only account-wide families, risk and counts at Dinis's word; Site__Agent__ABP f2188b5 history holds host names and truncated ids, no secret, nothing to rotate, no rewrite recommended; live page to reduce or come down.
- 19:24 UTC census post-check: Site__Agent__ABP 04bc64e content passes the never-list; prompt sha256 a7a1fdcc...e638 matches the pin. Live site not yet deployed (live prompt ae313675..., live JSON still names the mail connector): re-check after deploy. Still one agent's map: make account-wide or hold, at least drop hooks: 0.
- 19:31 UTC: census prompt live matches pin; 0a5c99b not yet live. b.abp lab hook-probes: probe table public OK; estate findings 1-4 to be sealed until fixed (unfixed weaknesses, key locations). Agreed with the four fixes.
- 19:34 UTC: b.abp live pages verified reduced (0a5c99b, c0d778b). Census content PASS. Four estate findings held privately (review 040) until fixed; post-check each, then publish.

## 20:35 UTC 25 Nov
- Dinis decided (b.abp's session, 20:32:55 UTC, verified): census and lab findings public in full. My ruling of 19:30 is superseded by his decision. No key or token is in them. Residual risk accepted by Dinis: the estate findings are unfixed and the census shows which session can send mail and steer others. I still post-check each fix.
- 20:41 UTC: b.abp fixes 1 and 2 (Site__Agent__ABP c7577ef, draft policy): PASS as a draft guardrail. Notes: add openssl pkey -in, ssh-keygen -y -f and input redirection; signing needs one allowed wrapper (the passphrase is read with cat); open until in the kit template and a running hook.
- 20:43 UTC: b.abp round 2 (70ab741): PASS as a draft; condition: tools/sign.sh and tools/setup-signing.sh must be protected paths. Open until in kit template and a running hook.
- 20:45 UTC: 7db478f protects tools/sign.sh and tools/setup-signing.sh: condition met; draft fixes 1 and 2 pass.
- 21:00 UTC: b.abp fix 4 first home (Site__Agent__ABP 370b78b): PASS as log-only. Notes: no breaker on (turn R3 on at Dinis's word); the lazy key-label fix belongs in the kit too. Landed when a session there shows the hook firing.
- 21:01 UTC: hook firing in b.abp's session, agent identified (log lines 20:56-20:57, no prompt text). Fixes 1, 2 and 4 landed for Site__Agent__ABP, log-only. Support R6 (.claude/**) and R3 breakers on; PR-1 ReadNotifications harmless.
- 21:05 UTC: Site__Agent__ABP ce9d094 (Dinis's word in b.abp's session): R3 breaker on, R6 on for tamper, PR-1 allowed: PASS. Gap for later: git checkout/restore/rm/apply on protected paths not seen as tamper.
- 21:07 UTC: Site__Agent__ABP e9a5bcb git tamper rules: PASS. Residual low: whole-tree git moves; suggested a SessionStart integrity check. Closed.

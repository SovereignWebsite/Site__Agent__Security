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

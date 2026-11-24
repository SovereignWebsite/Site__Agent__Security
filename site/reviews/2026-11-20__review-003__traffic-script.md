# Review 003: SovereignWebsite traffic script, draft 1 (pre-push)

- Reviewer: security.b@SovereignWebsite.dinis, 20:49 UTC 20 Nov 2026
- Owner: observability.b@SovereignWebsite.dinis (session_01Kw9UwDcJXSxNCBzPpxxXw8)
- Looked at: the design and the script (draft 1, about 90 lines) as sent in two messages. No repository or commit yet. I did not run it, and did not look at the SGit-AI newsroom original or the live SG/Send service.
- Dinis's decision, relayed by the conductor and taken as given: privacy note, no ask-first, no cookies, country/region/city only, no id that follows a person over time.

## Verdict: pass-with-changes

## Findings, worst first
1. High. The published stats page (and any aggregator) must treat every note as hostile. The append token and key are public, so anyone can write any note, including a city of "<script>...". Render only with textContent or an escape function, validate each decrypted note strictly (v == 1; site in the known list; path starts with "/" and is at most 200 characters; type in the four values; country two capital letters; region and city at most 80 characters of letters, spaces and . ' -; ts within an hour of the append time), and drop any that fail. Also guard any CSV export against formula injection (cells starting with = + - @).
2. Medium. The location lookup sends each visitor's IP address, and the site's origin (browsers send an Origin header on cross-origin fetches; no-referrer does not remove it), to freeipapi.com and, on failure, ipwho.is. That is personal data going to two outside companies without the visitor being asked. Preferred: get the country from the SG/Send side (for example a CDN country header on the append request), so no third party is involved. If that is not possible: use one provider, and the privacy note names it, says it sees the IP address, and links its privacy policy. Whether this is lawful without consent is Dinis's call; I am not giving legal advice.
3. Medium. Confirm in SG/Send that an append token can only append: it cannot read, list, delete or overwrite, and filling a lane (507) affects only that lane, not other vaults or the account's quota. Flooding cannot be stopped in the browser; the counts must be labelled as indicative on the published page.
4. Medium. CSP on every site: the script must be an external file (script-src 'self'), and connect-src must list exact hosts: 'self' (for /traffic.json), the SG/Send endpoint host, and the location host(s) only. The CSP is what stops a changed traffic.json sending notes elsewhere; site owners now push directly (Dinis, 20 Nov), so traffic.json is in their hands.
5. Low. Small counts: 5 and 28 days is reasonable for region and city, but differencing can undo it (for example a site total today minus a site total yesterday, or a total across sites minus the per-site totals). Publish city and region only as 28-day totals, never per path and never per day, and do not publish a total that equals another published total minus a cell under 5. Exclude type team and synthetic from published person counts.
6. Low. In the code: clip() takes the first characters of the provider's strings but not the characters themselves; add the character check from finding 1 here too. A visitor can set sw-synthetic=1 to remove themselves from person counts; fine, just document it. The 8-hex tab id, the seen list and the 40 cap are in sessionStorage and end with the tab, consistent with Dinis's decision; mention sessionStorage in the privacy note.

## Answers to observability.b's points
- (1) Yes, every record is an untrusted claim: findings 1 and 3.
- (2) Public append tokens and lane fill: finding 3.
- (3) Third-party location and connect-src: findings 2 and 4.
- (4) 5 and 28 days: finding 5.
- (5) Code: findings 1 and 6. DNT and GPC, https-only, host list, credentials omit, ip never read and the envelope are fine as written.

## Cost
- Not yet reported; not estimated.

## After, 20:51 UTC
- observability.b reports draft 2: finding 6 done (clip character check, country two capitals); one provider, freeipapi.com; findings 1, 4 and 5 in the design; privacy note names freeipapi.com, what it sees, sessionStorage, counts indicative. Finding 2 preferred option and finding 3 with hub.b; 3 to be confirmed by test in the pilot. Finding 2's lawfulness with Dinis. Not seen by me; to be checked on the commit. Verdict stays pass-with-changes; nothing live before the commit check.
- 16:00 UTC 23 Nov: draft 3 design (no third-party location; note adds tz and lang, pattern-checked; drain maps tz to country, publishes countries only, min 5). Findings 2 and 5 retired. Asked: unknown for tz without a country, finding 1 validation on tz and lang. Finding 3 open (owner of SG/Send not named). observability.b cites a decision record (not verified by me; the change reduces risk, so it does not need verifying for this review). Commit awaited.
- 23:18 UTC 23 Nov: finding 3 answered from SG/Send's own documentation (cli.sgit.ai/api/append-lanes.html; SGit-AI__Website__Cli fe9f2d5): append token writes only to its own lane, cannot list/fetch/read/delete/configure; 5 MB payload, 1000 pending per token (507), lanes isolated per token; no rate limit documented. Accepted on the documentation, to be confirmed by test on the pilot vault with the commit. Note: no rate limit, so flooding fills a lane to 1000; counts stay labelled indicative.
- 01:13 UTC 24 Nov: finding 3 tested on the real traffic vault (made by b.maestro, block 05): append token writes (200) but cannot list (404), fetch (400, malformed id) or purge (401); enum key lists, fetches, marks processed; decrypt works. Closed, with one check to finish: fetch with the append token and a real file id. Pilot commit awaited.

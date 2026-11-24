# Review 025: South Beacon anonymised talent profile, re-identification risk (pre-live)
- Owner: south-beacon.b@SovereignWebsite.dinis (session_01Kw9UwDJPFyYWmBHRfqYodU). Reviewed 14:31 UTC 21 Nov from the page text in its message only; no searching for the person.
- Verdict: PASS-WITH-CHANGES. No single line identifies; the combination narrows.
  1. High: achievement 2's two internal products (may be presented or published); generalise; ask the candidate.
  2. High: "small founding information-security team grow" dates the person; generalise.
  3. Medium: "major enterprise cyber incidents" may tie to a public incident; generalise.
  4. Medium: energy/utilities/CI consulting then large enterprise, OT plus cloud; keep one sector phrase.
  5. Low: endpoint protection major investment (vendor case studies); generalise.
  6. Low: specific practices in achievement 3; consider merging.
  Outside the text: git history, file metadata, sealed form, page metadata.
- 14:34 UTC: south-beacon.b reports Dinis chose all changes; wording applied as suggested (items 1 to 6). Checks a to d to be done before first push; final look on the live address to follow.
- 14:46 UTC final look, live https://south-beacon.talent.sovereignweb.site/ and repo Site__Talent__South-Beacon (3 commits: bot initial, bot seed, south-beacon.b 53ed671):
  - None of the removed phrases appear in any site file or anywhere in git history (git log --all -p).
  - Meta description and og tags generic. Only image: the seed's 5,860-byte share card, no EXIF/author fields.
  - Form: browser keys non-extractable in IndexedDB; sealed to the talent agent's key; keeps the visitor's own sent-item preview (role and organisation) in localStorage, with a remove; low: say so on the form.
  - Home, introduction and four design pages answer 200 (designs/ index gave 503 once; the four design pages 200 on retry).
  - Review 025 CLOSED: PASS. Residual: any CV-style profile stays partly recognisable to people who know the candidate; the candidate accepts that by publishing.
- 14:58 UTC delta: 09bae0d Dossier bars are empty same-width spans (8rem) with '(withheld)', no real values in source; bcf8fa2 Letter in first person (checked diff for added facts). c25d467 records candidate approval (Dinis). PASS stands.
- 15:00 UTC: low: comment in src/profile.mjs (bcf8fa2) quotes 'his voice', revealing gender in the public repo; asked to reword; history copy accepted unless Dinis wants it rewritten.
- 15:01 UTC: 55e765a reworded; grep of current site/src/docs for gendered pronouns.

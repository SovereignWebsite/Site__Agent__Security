# Review 009: Site__Provider__Mistral site/browser/keys/ (browser key vault, pre-push)

- Raised by conductor.mistral 01:46 UTC 21 Nov, at Dinis's request (Dinis's design, relayed). Reviewed from its description; code not read.
- Verdict: PASS-WITH-CHANGES; push once 1 to 3 done.
  1. Medium: expiry of the stored sealed key (inside the envelope; refuse and delete after).
  2. Medium: origin binding: seal page refuses foreign-origin public envelopes; target refuses foreign sealed envelopes; origin and fp as AES-GCM additional data.
  3. Low: bold "only seal for a browser in front of you; compare fingerprints"; large fingerprint on both pages.
  4. Residual for Dinis: shared origin; consider a separate origin or removal after the hackathon.
- Holds: non-extractable RSA-OAEP 3072 private key made on click; ciphertext only stored; store only on the owner's yes; fp check; vault loaded on press only; key into the existing closure; no network on keys/ or seal; frame check; residual risk stated on the page.
- 01:56 UTC post-check (936bea8, in dev at bda7192): expiry in the envelope and refused/deleted when past; origin checked on both envelopes; AES-GCM additional data binds v, origin, fp, created, expires; RSA key non-extractable; frame check in every script; no fetch, XHR, HTML sinks or web storage in keys/; CSP connect-src 'self'; bold "in front of you" warning present. Review 009 CLOSED: PASS. Residual (shared origin) with Dinis.

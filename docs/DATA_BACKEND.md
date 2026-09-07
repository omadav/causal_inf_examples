# Data backend: Web3Forms

All 3 pages (`module-a.html`, `module-b.html`, `module-c.html`) POST each
submission as JSON to Web3Forms' API:

```
POST https://api.web3forms.com/submit
Content-Type: application/json

{ "access_key": "<the shared access key>", "modulo": "A|B|C", "payload_json": "<JSON string of that module's answers>" }
```

Unlike the earlier Google Forms approach, this returns a real, readable
`{ success: true/false, message: ... }` response — so a failed submission
is now detectable, and the page shows the student a visible warning
("No se pudo enviar") instead of a false "success" screen.

**Every submission also downloads a local CSV backup** to the student's own
device (one row, their own answers, filename `modulo-<A|B|C>-<id>.csv`),
regardless of whether the Web3Forms send succeeds. This is a safety net,
not the primary data path.

## Retrieving responses for analysis

Web3Forms' free tier stores submissions in its dashboard (30-day retention)
and supports exporting to CSV from there, but **pulling submissions
programmatically requires a Pro plan** — so the flow is:

1. Log in at [web3forms.com](https://web3forms.com/) → your form → **Submissions**.
2. Export to CSV.
3. Send that file here (or drop it in `analysis/data/responses.csv`) —
   `analysis/session.ipynb` reads it, splits by the `modulo` column, and
   expands `payload_json` into each module's fields (same as before).

## Limits

Free tier: 250 submissions/month, account-wide (not per form) — comfortably
covers a single class session across all 3 modules.

## Access key

The key embedded in the 3 pages is a **public** key — Web3Forms' own docs
say it's safe to expose in client-side code (that's the whole design: it
identifies the form, not a secret credential).

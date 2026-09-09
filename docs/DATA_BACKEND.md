# Data backend: Web3Forms

Every page POSTs as JSON to Web3Forms' API:

```
POST https://api.web3forms.com/submit
Content-Type: application/json
```

Two submission shapes exist, both landing in the same Web3Forms form/CSV:

**Individual pages** (`module-a.html` … `module-d.html`) — one module per
submission:

```json
{ "access_key": "<the shared access key>", "modulo": "A|B|C|D", "payload_json": "<JSON string of that module's answers>" }
```

**`flow.html`** (all 4 modules in one sitting) — one submission for all 4
modules, since firing 4 separate POSTs within a few seconds got the later
ones flagged as spam by Web3Forms (confirmed with a real test: submissions
still came back `success: true`, but the flagged ones never showed up in
the CSV export — no error, no warning screen, the data just silently never
arrived):

```json
{
  "access_key": "<the shared access key>",
  "modulo": "FLOW",
  "payload_a_json": "<Module A answers>",
  "payload_b_json": "<Module B answers>",
  "payload_c_json": "<Module C answers>",
  "payload_d_json": "<Module D answers>"
}
```

`analysis/session.ipynb`'s `load_module()` reads both shapes: it collects
rows where `Modulo` matches the module letter (individual pages) *and*
rows that have a non-empty `payload_<letter>_json` column (`flow.html`),
then expands whichever JSON column applies into that module's normal
columns. Every field name inside each module's payload is identical
between the two shapes, so downstream analysis code doesn't need to know
which page a given row came from.

A real, readable `{ success: true/false, message: ... }` response (unlike
the earlier Google Forms approach) lets a page detect an actual rejected
submission and show a visible warning ("No se pudo enviar") instead of a
false "success" screen — but note the caveat above: a submission Web3Forms
silently flags as spam still reports `success: true` to the page, so this
detection only catches network/validation failures, not spam-filtering.

**If a submission fails** (network error, or Web3Forms returns
`success: false`), the page downloads a local CSV backup to the student's
own device as a fallback — filename `modulo-<A|B|C|D>-<id>.csv`. This only
happens on failure, not on every submission.

## Retrieving responses for analysis

Web3Forms' free tier stores submissions in its dashboard (30-day retention)
and supports exporting to CSV from there, but **pulling submissions
programmatically requires a Pro plan** — so the flow is:

1. Log in at [web3forms.com](https://web3forms.com/) → your form → **Submissions**.
2. Export to CSV.
3. Send that file here (or drop it in `analysis/data/responses.csv`) —
   `analysis/session.ipynb` reads it and expands each module's fields
   (see above).

## Limits

Free tier: 250 submissions/month, account-wide (not per form). Since
`flow.html` now sends 1 submission per student instead of 4, a full class
session comfortably fits well under that.

## Access key

The key embedded in the pages is a **public** key — Web3Forms' own docs
say it's safe to expose in client-side code (that's the whole design: it
identifies the form, not a secret credential).

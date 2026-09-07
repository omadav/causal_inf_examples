# (Deprecated) Google Forms backend

**This approach was replaced.** We confirmed in testing that a real, one-off
submission to the Google Form's `formResponse` endpoint via
`fetch(..., {mode: 'no-cors'})` was silently rejected server-side — with
`no-cors` the response is opaque, so there was no way to see the actual
error. Rather than keep debugging a fragile reverse-engineered trick, the
3 pages now use **Web3Forms** instead — see `docs/DATA_BACKEND.md`.

This file is kept only as a record of what didn't work and why.

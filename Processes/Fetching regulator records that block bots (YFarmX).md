---
tags: [process, yfarmx, research, sourcing]
source: The ten reference pages session, 18 September 2026
updated: 2026-09-18
---

# Fetching regulator records that block bots (YFarmX)

Policy and regulation pages live on primary documents, and half the agencies
serving them block plain fetches. These are the routes that worked on
18 September 2026, found while building the ten policy reference pages. Each
one saves a future session an hour of guessing.

## The routes, agency by agency

- **FCC satellite filings**: `fcc.report` is Cloudflare-gated end to end and
  the main fcc.gov pages Akamai-block curl. What works: the FCC's own ICFS
  Angular portal at `fccprod.servicenowservices.com/icfs` (Playwright through
  the agent proxy), and its attachment API at
  `api-prod.fcc.gov/icfs-attachment/exp/api/v1/<id>` for the filing PDFs.
  Daily Digest PDFs at `docs.fcc.gov/public/attachments/DOC-…` fetch plain.
- **bis.gov**: the press pages are Next.js; the content sits in the embedded
  `__NEXT_DATA__` JSON, so extract that rather than rendering. The licence
  exception table at `bis.gov/iec` serves HTML to a browser but returns the
  actual PDF to `curl -H "Accept: application/pdf"`.
- **sec.gov**: 403s undeclared tools and says so in the block page. A declared
  identifying User-Agent (product name plus a contact URL) is what its policy
  asks for, and with one set, EDGAR filings and rule PDFs fetch cleanly.
- **ferc.gov**: Cloudflare-challenged for curl, fine in real Chromium through
  the proxy. eLibrary docket sheets are fetchable and carry filing dates,
  which is how the red team caught a wrong abeyance date.
- **openai.com and perplexity.ai**: hard Cloudflare challenges to this
  container in every form tried, including their help centres and the Wayback
  replays of their Next.js pages (client-side exceptions). Perplexity's hub
  blog posts and `openai.com/news/rss.xml` are the readable edges. Plan
  product screenshots around vendors that serve them (claude.com and
  anthropic.com both do).
- **Wikimedia Commons**: the API rate-limits this proxy IP quickly when
  several agents share it. Space the calls, send a descriptive User-Agent,
  and fetch image originals rather than thumbs when the thumb 404s. Check
  `extmetadata` for the licence before using anything: CC0, CC BY and public
  domain only, per decision 75.

## Cutting figures out of the PDFs

`pdftotext -bbox` gives word coordinates in page points; multiply by dpi/72
and hand the box to `pdftoppm -x -y -W -H -r 300`. That turns "crop the
paragraph from 'SUMMARY' to 'China or Macau'" into a repeatable command
instead of pixel guessing, and it is how all the Federal Register and SEC
release crops on the ten pages were cut. The session helper was
`scratchpad/crop.mjs`; rebuild it from this note when needed, it is thirty
lines. Scanned PDFs (the AB 1064 veto letter) have no text layer: render the
page and verify quotes by eye.

## The browser recipe

Chromium at `/opt/pw-browsers/chromium-1194/chrome-linux/chrome` with
`--proxy-server=$HTTPS_PROXY --ssl-version-max=tls1.2` and the proxy CA added
to the NSS store (`certutil -d sql:/root/.pki/nssdb -A -t "C,," -n ccr -i
/root/.ccr/ca-bundle.crt`). Verification stays on; never
`ignoreHTTPSErrors`. This is the same recipe as `scripts/screenshot.mjs`, and
it held for EUR-Lex, leginfo, senate.gov, nvidia.com and the FCC portal in
one pass.

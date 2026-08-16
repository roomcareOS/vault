---
tags: [process, yfarmx]
source: yfarmx/docs/space/RECON.md, yfarmx/docs/space/MODELS.md, yfarmx/docs/space/BUDGET.md, yfarmx/docs/space/IMAGE-PROMPTS.md, yfarmx/docs/space/IMAGE-PROMPTS-26.md, yfarmx/docs/space/HUB-COMPLETION.md, yfarmx/docs/space/OUT_OF_SCOPE.md
updated: 2026-08-16
---

# Space Hub Build (YFarmX)

The Space section of [[YFarmX]]: a hub, sixteen topic desks (News, Launches, Missions, Satellites, Orbit, Moon, Mars, Science, Robotics, Business, Security, Policy, Defence, Analysis, Opinion, Data) plus an About page, all under `/space/`. **LAUNCHED 7 August 2026** — Jay flipped `SPACE_PUBLIC` by voice instruction that evening (it was his call alone, and remained so; see [[Decisions - Space Hub (YFarmX)]]). The world now carries its own display face (Chakra Petch, loaded only on space pages), decode-on-view headings, a two-world masthead switcher, per-desk banner art on all sixteen desks, and a sister-title band on the main homepage.

## The working method (this is the reusable part)

The build ran as a disciplined agent project, and the shape is worth copying for any future section:

1. **Phase 0 is read-only recon.** Inspect everything, change nothing, write it up (`RECON.md`) — including where the spec and the code disagree, stated plainly, because each disagreement changes the size of a task.
2. **Translate the scope lock, on the record.** The spec assumed a `space/` directory that does not exist; taken literally it forbade every file it asked for. The recon proposed an editable "Space set" and an off-limits list, and asked Jay to confirm rather than deciding silently.
3. **Keep four standing documents:** `RECON.md` (what exists), `DECISIONS.md` (one line per non-obvious choice, with the reason, newest last), `OUT_OF_SCOPE.md` (an append-only register: every request the lock forbids, with the smallest diff that would have satisfied it), `BUDGET.md` (performance numbers **measured, not estimated**, with the method stated and dated blocks added whenever the section changes shape).
4. **End every phase with numbered questions for Jay.** Nothing invasive happens without an answer.

## Honesty rules that shape the whole section

- **No invented data, anywhere** — the About page promises it. Every 3D model is a *class archetype* ("communications satellite"), never captioned with a mission name; every manifest entry carries provenance `illustration`.
- The launch countdown renders only for confirmed windows (a date-only window gets no clock).
- Empty desks render an honest empty state, not fake content.
- **Opinion runs under a named byline, never the house byline** — signed argument needs a person behind it. (Until 8 Aug 2026 the rule was that Opinion stayed empty until Jay wrote it; he lifted that himself and the desk now runs pieces under the founder's byline.) News needs no article of its own; it renders the whole Space wire.
- Data files carry per-record `sources` and `lastVerified`; "NEVER add a launch without primary sources" is written into the file itself.

## The 3D model library

Sixteen Blender-built class archetypes. **The Python scripts are the source** (`scripts/space3d/`); no binary `.blend` files are committed, so a git-based newsroom can diff and review its own artwork. Each model outputs 8 turntable frames (WebP), a poster, a GLB master (never served to readers — too heavy) and a metadata sidecar folded into a manifest. Scale is **measured by script, not typed by hand**, after a geometry-helper defect made hand-typed figures untrustworthy.

Adding a model, as a checklist:
- [ ] Copy `models/sat_comms.py` (the reference); define `SPEC` and `build()`
- [ ] Keep to the material palette; model in metres; colour only as functional accents
- [ ] Iterate with `--preview` (one small fast frame) until it reads from every angle
- [ ] Run `build_all.py` to render and fold it into the manifest

## Performance discipline

Measure with a stated method, then attack the biggest number first. The case study: the hub loaded 3,558KB on a phone against a 2MB budget, and one hero video was 2,529KB of it. A moderate denoise before re-encoding cut it 79% with no visible damage (checked frame against frame); a phone rendition took it to 353KB. Modern codecs were **tested and lost** on this footage, so the spec's WebM rule was deliberately not followed — measurements beat rules. By 2 August the hub's initial mobile transfer was **595KB**, with the 3D viewer loading lazily and costing no GPU while idle.

## Artwork rules (26 prompts written and waiting)

Two rules override every image prompt:
1. **No invented data in artwork.** Every figure in an infographic must be one the article carries and the source states. `[FROM ARTICLE]` placeholders must be filled from the reporting, never by the image tool. (The cautionary case: a shipped infographic carried a rating no source stated.)
2. **No decorative chart shapes.** An unlabelled bar chart is filler that looks like evidence.

And an ordering rule: **write the article before the hero prompt** — art that predates the reporting invents the picture, and the copy then gets written to match it.

## Launch blockers (as recorded 30 July – 2 August 2026)

- [ ] Articles: ten desks empty (four drafts written: Science, Security, Mars, Moon)
- [ ] Art: no article hero exists; six desks share banners; no image key in the build environment
- [ ] Audio: none — the playbook treats audio as part of the article
- [ ] Jay's review: drafts stay `draft: true` until Jay reads and edits them (the only stop in the loop, same as the [[Article Pipeline (YFarmX)]])
- [ ] CLS (layout shift) 0.0588 against a target of zero — untraced; font swap is the hypothesis, not the finding

The honest summary from the completion doc: the machinery works; what the section lacks is journalism, and the next unit of work is articles, not features. (The [[YFarmX]] status snapshot records the Space desk's first live news article on 5 August.) **Post-launch note, 7 Aug:** the shared-banner art blocker is resolved (every desk owns its banner); empty desks still render their honest empty states, and articles remain the real work.

## Mission imagery (8 Aug 2026)

Eleven of the twelve missions on the Missions desk now carry real imagery of the actual craft, restyled onto the house starfield (Chang'e-7 ships imageless — no cleanly licensed source exists; Haven-1 is unmodified because Vast's press terms forbid alteration). The class-archetype rule above still governs the **3D models**; photographs of the real, named craft are a different category and are captioned and credited as such. The sourcing, licensing and restyle workflow is recorded in [[Image Style and Prompt Libraries (YFarmX)]] — read it before adding any mission or hardware image. One later variant (same day): when the image IS the phenomenon rather than an object — NOIRLab's Starlink trail photograph — it ships unmodified (crop only), because a restyle would erase what it evidences.

## The desks grew real boards (8 Aug 2026, evening)

The mission ledger stands at **19 records** and every desk that lists missions carries at least three (Jay's floor). Mission cards are compact by rule — image, name, tags, a one-or-two-sentence `brief` — with the full record behind a native More expander. Two desks stopped being empty: **Analysis** is a dated analyst dashboard (`space-analysis.json`: launch cadence on one stated methodology, the market board with a standing not-investment-advice line, 90-day milestones and deals, orbit counts) and **Business** is the company boards (`space-business.json`: startup watch with company-stated claims labelled, plus the established layer). Both carry a **weekly refresh duty registered in `docs/time-sensitive.md`** — the registry, not this note, owns that cadence. New honest phase label: `data-processing` (Gaia — spacecraft retired, archive live).

## Two rules learned the hard way (8 Aug 2026, evening)

**A standfirst and an explainer that say the same thing make the page look broken.** Every one of the sixteen desks opened its explainer by restating its own standfirst, so each desk printed its purpose twice in different words. Jay caught it on Opinion and Defence. Rule: the explainer earns its place only by saying something the standfirst does not.

**Do not explain the methodology on the page; state the fact and cite the source** (Jay, 8 Aug: "don't explain our methodology! just tell the fact"). Removed from the Analysis desk: a standing not-investment-advice line, a paragraph explaining how the launch tables were compiled, and a "no recommendations" tag. The sourcing link stays, the caveat in a row's own note stays, the essay about our process goes.

**Opinion now runs**, superseding the stays-empty rule by Jay's direct instruction (8 Aug). It runs under the founder's named byline, never the house byline, and each piece argues from reporting already verified on the desk. **Defence** is a by-country capability board where every line carries a confidence label: `verified`, `announced`, `assessed` (someone else's estimate, attributed to them) or `speculation`/`disputed`. The labels are the honesty mechanism that lets the desk carry contested claims at all: Russia's orbital nuclear weapon appears with both the US assessment and Putin's denial, and China's ISR total is marked as a US figure rather than a Chinese one.

## Deep links: every record has an address (8 Aug 2026, late)

The hub and the desks share one anchor scheme, and it is load-bearing. `spaceAnchor(name)` in `src/lib/space-sections.ts` turns a mission or launch name into a slug; every desk record renders `id="m-<anchor>"` and every launch window `id="l-<anchor>"`. **Never hand-write a link to a record**: `missionRecordHref(name)` in `src/lib/space-desk-content.ts` returns the desk page that actually renders the record (missions desk first, otherwise the first desk listing it in `DESK_CONTENT`) — a hard-coded `/space/missions/#m-…` dangles the moment the record lives elsewhere, which is exactly what happened to GPS III and the Starlink constellation (satellites desk only). Renaming a mission in the data changes its anchor: in-site links self-heal on the next build because they all derive from the same helpers, but external links to the old hash die. A built-site sweep for dangling record anchors is cheap and worth re-running after data edits.

**The ring rule (Jay, engraved 8 Aug from live use): the blue ring marks state the reader controls, never the visited link.** On record and startup cards it tracks the card whose More is open — one open per board via the `details name` attribute, so opening a card closes the last, and closing More removes the ring. Launch-window rows have nothing to unfold, so they are a click selection: the ring follows each click and a second click on the selected row switches it off. Arrival on a `#m-`/`#l-` hash opens (or selects) the cited record, which is what rings it; a `:target` ring is banned because it outlives the interaction. Companion rule: clicking a record link whose hash is already in the address bar fires no hashchange, so the opener re-runs on record-link clicks — a click on this estate must never look dead. The hub's launch board is the same idea in accordion form — five `<details name="launchboard">`, first open, red accent on the open one. On phones the row summary is two rows (pill and window above, name below) because a nowrap window column starves the name — checked at 375 px before shipping.

## The market tape (8 Aug 2026) — the house pattern for live data on a page

Jay asked for the top space stocks with live prices at the top of `/space`,
"always working, whatever the traffic or the rate limits". The shape that
satisfies it is the estate's standing pattern for ANY live third-party number,
already proven by `/api/prices` for crypto, now applied to equities as
`/api/markets` (yfarmx decision 49 has the full reasoning):

1. **Visitors never talk to the provider.** One Pages Function does, and
   `cf.cacheTtl` dedupes that to roughly one upstream refresh a minute per
   edge no matter how many readers are on the page. Collect once,
   redistribute.
2. **Three layers, so nothing ever blanks:** a build-time snapshot
   server-rendered from a committed data file (JS off or endpoint down still
   shows honest figures with their provenance stamp), a live upgrade in
   place once a minute, and a last-known-good copy at the edge for provider
   wobbles.
3. **All live or all snapshot, never a mix.** The endpoint refuses partial
   sets, and the committed file refuses partial refreshes, for the same
   reason: a strip mixing fresh and stale rows lies about its own timestamp.
4. **One symbol list.** The Function imports it from the same committed file
   the page bakes from, so the three layers cannot disagree about coverage.
5. **The LIVE indicator is earned, not asserted:** grey until live figures
   are actually on screen, and the stamp names which state is showing.

**Two Yahoo chart-API traps, guarded in code but worth knowing on sight
anywhere Yahoo data appears:** `meta.chartPreviousClose` is the close before
the *requested range* (on a 1mo call that is a month ago — a day change
computed against it is silently a month change); and the quote field
`meta.regularMarketPrice` can lag Yahoo's own chart by days while carrying a
fresh session timestamp (QBTS, 8 Aug 2026: quote $16.21, chart series $20.76,
Nasdaq $20.76). Price from the daily close series; trust the quote field only
while it agrees with the series.

## Verifying launches: read SpaceX's own CMS, not its website (16 Aug 2026)

`www.spacex.com` serves an Angular shell — plain `curl` returns about 3KB of
`<app-root></app-root>` and nothing else. The 14 August pass concluded from this
that the manifest needs a rendered browser, and wrote that into the data file.
**It does not.** The shell is fed by SpaceX's own CMS, which answers plain HTTP
with no key, no browser and no Playwright:

| Endpoint (on `https://content.spacex.com/`) | What it gives |
|---|---|
| `api/spacex-website/launches-page-tiles` | every launch ever: date, launch time, pad, vehicle, mission status, return site |
| `api/spacex-website/launches-page-tiles/upcoming` | the forward manifest (often undated) |
| `api/spacex-website/missions/<slug>` | the post-flight prose, booster flight history and full launch/landing timeline |
| `api/spacex-website/falcon-nine-stats` | all-time Falcon 9 launches, landings, reflights |

The endpoint list is discoverable at any time by grepping the site's `main.*.js`
bundle for `api/spacex-website`. Two things to know when using it:

- **`launchTime` is Eastern, whatever coast the rocket left from.** A Vandenberg
  launch reads `21:50` for a 6:50 p.m. Pacific liftoff. Convert deliberately;
  the mission page's own prose states the correct local zone and is the check.
- **The tiles repeat.** The same launch appears as a homepage tile and a launches
  tile. Dedupe on `link|launchDate` before counting anything. Done that way the
  feed is an authoritative launch count — better sourced for the Analysis desk's
  cadence table than the Wikipedia tables it was built from.

**The date trap that comes with this.** Both 16 August Falcon 9 flights lifted off
after midnight UTC and every American source files them as *15 August*. The launch
board publishes UTC, and UK readers read UTC. Check which day a launch falls on in
*our* reckoning before writing it up — the previous pass pasted the manifest's
Eastern clock time straight into a UTC field and put USSF-366 four hours out, on
the wrong day.

**A flown launch is not a window.** Records stay on the board for 24 hours after
liftoff so the outcome is readable the next morning. Anything forward-looking —
"next window", "windows tracked", "in the next 30 days" — must filter
`launched` and `failed` out first, or the hub advertises a rocket that has already
gone as the next one up. Found live on `/space` on 16 August; the same defect had
been showing since QZS-7 flew on the 11th.

## Traps the 7 August adversarial review caught (worth remembering)

Twenty-four confirmed findings, all fixed before the production push. Three generalise beyond this build:

- **A failed dynamic `import()` is memoised by the module map** — retrying the same specifier rejects instantly from cache without touching the network, so an in-code retry of a chunk import is largely theatre. Design the fallback (here, the sprite drag) rather than the retry.
- **`@fontsource` imports in a layout leak beyond it.** The `[slug]` article shell imports both world layouts statically, so a font CSS import in SpaceBase became a render-blocking stylesheet on 500-plus article pages that never draw the face. A hand-written `@font-face` over a self-hosted woff2 in the world's stylesheet costs nothing until an element computes to the family.
- **Treat a superseded async load as benign, never as the machinery failing.** A picker click during the viewer's boot rejected boot's own load with 'superseded', and one catch-all marked WebGL dead for the rest of the page view. Separate "the machinery broke" from "a newer request overtook this one" in every loader.

[[Map - Processes]] · [[Decisions - Space Hub (YFarmX)]] · [[Hermes Newsroom Pipeline (YFarmX)]] · [[Image Style and Prompt Libraries (YFarmX)]] · [[Robotics Launch Checklist (YFarmX)]]

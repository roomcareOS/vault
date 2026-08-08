---
tags: [process, yfarmx, security]
source: security hardening pass, 8 August 2026 (branch claude/website-hardening-ip6ike)
updated: 2026-08-08
---

# Pipeline Security Rules (YFarmX)

The published site is not where [[YFarmX]]'s risk lives. A hardening pass on 8 August 2026 swept the workflows, the client JavaScript, the edge layer and the tree, and found the tree clean of credentials and the pages genuinely well defended: every third-party number that reaches the DOM is turned into a number first, and every string a reader types goes in through `textContent`. Everything worth fixing was one layer behind the pages, in the machinery that publishes them. These are the rules that came out of it. They bind every session and Hermes equally.

## 1. A secret goes on the step that uses it, never on the job

A job-level `env:` block puts the secret in the environment of **every** step, and one of those steps is `npm ci`, which executes install scripts from the whole dependency tree. `social.yml` held fifteen platform credentials that way: X, LinkedIn, Telegram, Discord, Bluesky and Mastodon, all readable by any transitive package on any scheduled run.

Step-level `env:` costs a few repeated lines and removes the entire class. Repeat the block on both steps rather than hoisting it.

## 2. Every workflow declares `permissions:`, and a non-pushing job also drops the token

Three workflows (`deploy`, `deploy-staging`, `buffer-reschedule`) declared nothing, so they ran on the repository default. None of them commits, yet each runs `npm ci` while `actions/checkout` leaves the workflow token in `.git/config`. That is a possibly write-scoped token on disk next to arbitrary install scripts: one bad package pushes to `main` and owns the site.

- Declare `permissions:` on every workflow. `contents: read` unless the job genuinely commits.
- On any checkout in a job that never pushes, set `persist-credentials: false`.
- **A `permissions:` block replaces the defaults wholesale, it does not add to them.** `markets.yml` carried a `gh workflow run` step that was refused on every single run for as long as it existed, because `contents: write` alone left no `actions` scope, and the `|| echo` hid it. Check the scopes a step actually needs against the block, or the step is decoration.

## 3. `git push` in a retry loop must fail the run when it never succeeds

The pattern `for i in 1 2 3; do git push && break; git pull --rebase ...; sleep 3; done` exits **0** when every attempt fails, because the loop's last command is `sleep`. Four workflows had it.

The consequence is specific and expensive: the social and buffer workflows commit the queue file back to record what was posted. When that push is lost the queue stays at `pending`, the next `*/15` cron reads it, and **the same article posts to X and LinkedIn a second time** with nothing red anywhere to notice. Set a flag inside the loop and `exit 1` if it is still unset. A post that went out but was not recorded is exactly the situation that must be loud.

## 4. `public/_headers` does not reach a Pages Function

`_headers` is applied by the Pages static-asset layer, and a Function response never passes through it. So `/api/publish` and `/api/prices` shipped with none of the site's header set, despite a thorough `_headers` file sitting in the repo.

Any new API route must set its own headers in code. There is no root `functions/_middleware.js` on production and there should not be: one is copied in **at CI time only** by `deploy-staging.yml` to gate staging (see [[Staging and Backups (YFarmX)]]), and a committed one would put a Worker in front of every request on the live site, cost included.

## 5. localStorage is an input, not a store you own

Three tools took a value back out of `localStorage` and handed it to a sink that trusts it: the gas fee checker cached `cardsEl.innerHTML` and replayed it into `innerHTML` on every boot, the command palette navigated to whatever URL a Recent row carried, and the crypto quiz picked badge art with a truthy lookup (which every `Object.prototype` key answers) and interpolated the id into `innerHTML`.

None was exploitable on its own, and that is the reason to care rather than a reason to shrug: each one converts a transient compromise anywhere on the origin into a **permanent** one that survives every reload, behind a strict CSP that stops script but not markup, styling or a convincing link. The rules:

- **Cache data, never markup.** An id-to-text map restored with `textContent` does the same job and cannot carry HTML. Bump the cache key when the format changes, so a payload already on a reader's phone is ignored rather than parsed.
- **A stored URL is validated before it is navigated to.** The palette only ever goes to pages on this site, so a destination must be a root-relative path (`/` and not `//`). Rebuild stored rows with only the fields the code reads.
- **Look up with `hasOwnProperty`, not truthiness**, whenever the key came from outside.
- **Parse defensively.** A malformed value in `yfx_badges_v2` threw at the top of the quiz script and took the whole game down. Wrap the parse, check the shape.

## 6. The write surface needs a throttle, and it has none

`POST /api/publish` is the only writable surface: bearer token in `DESK_PASSWORD`, used by the Desk and by Hermes, committing straight to the repo through the GitHub API. The auth itself is sound (SHA-256 digests compared in constant time, fails closed when unconfigured), and it now carries ceilings on body size and on the tags, keyPoints and sources arrays so a runaway loop cannot commit a file that breaks every later build.

What it does not have is a rate limit, so the token can be attempted without cost. That is a Cloudflare dashboard rule and therefore Jay's click, and it is on his board. Note the endpoint validates its slug and image paths tightly enough that path traversal is not the exposure; the exposure is the token.

## 7. Keep the dependency tree quiet

Seven advisories (one moderate, six high) were open at the time of the pass, all transitive: `undici` under wrangler/miniflare, `postcss` under astro/vite. All cleared by `npm audit fix` as a lockfile-only change with the build passing end to end. Run `npm audit` as part of any hardening or dependency work; a clean tree makes the rare real finding visible.

## Still open

- **`cloudflare/wrangler-action@v3` is pinned to a mutable tag** and receives the production Cloudflare deploy token. It wants a commit SHA. A guessed SHA breaks every deploy, so it needs a session that can read that repository's refs. The GitHub-owned `actions/checkout` and `actions/setup-node` are lower risk and left on tags deliberately, to keep the maintenance load on a solo operator honest.
- **`backup.yml` publishes a weekly `git bundle --all` as a release.** Harmless while the repo is private, since release visibility follows repository visibility. If the repo is ever made public, those bundles carry every branch and all history, including unreviewed staging content. Check this before any decision to open the repo.

## Links

[[YFarmX]] · [[Map - Processes]] · [[Staging and Backups (YFarmX)]] · [[Social Syndication (YFarmX)]] · [[Article Pipeline (YFarmX)]] · [[Session Doctrine]]

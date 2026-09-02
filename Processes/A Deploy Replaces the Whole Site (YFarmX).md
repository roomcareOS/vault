---
tags: [process, yfarmx, deploys]
source: the 1 September 2026 deploy that deleted eighteen live pages, and the 2 September restoration
updated: 2026-09-02
---

# A Deploy Replaces the Whole Site (YFarmX)

A Cloudflare Pages deploy is **a full snapshot, not a patch**. Whatever is in `dist` becomes the entire site, and every page absent from it is deleted. [[YFarmX]] deploys by hand (`wrangler pages deploy dist --project-name yfarmx --branch main`) because Actions has been dead since 28 August 2026, and hand deploys are where this bites.

On 1 September a session deployed `dist` built from its own feature branch. That branch was ten commits behind `main` and knew nothing about a second branch where another session had written seven articles. **Eighteen live article pages were deleted.** The build was clean and reported success, because it built exactly what its branch contained.

## Why it hid for a day: the edge cache lies about it

Cloudflare's edge went on serving cached copies of the deleted pages. So:

- every deleted article URL still answered **200** when clicked, from a post on X or anywhere else
- the homepage, `/news/` and `rss.xml`, which rebuild from the content collection, had already dropped them

Jay reported it as *"the links work, but they don't show on the home page"* — which reads like a **sorting bug** and sends you into the homepage's date handling. It is not a sorting bug. It is deletion, wearing a cache as a disguise.

A plain `curl` confirms the wrong thing. **Only a cache-busting query string reaches the origin:**

```
curl -s -o /dev/null -w '%{http_code}\n' "https://yfarmx.com/<slug>/?cb=$RANDOM"
```

Four sampled slugs: **200 cached, 404 at origin.** That single comparison is the whole diagnosis, and it takes ten seconds. Run it before theorising about anything else.

## The gate

`scripts/predeploy-check.mjs` (`npm run predeploy-check`) reads the **live sitemap** and refuses any build missing a URL the site currently serves. Run it after `npm run build` and before `wrangler pages deploy`.

Two design points worth keeping if it is ever rewritten:

- **It fails, rather than passes, when the live site cannot be reached.** A gate that waves builds through whenever the network is down is worse than no gate, because it gets trusted.
- **Deliberate removals are named:** `--allow-drop=/old-page/`. Unpublishing stays possible, but as a decision rather than an accident.

Prove any such gate in **both** directions before trusting it. The first control run here was a false pass: the page removed from `dist` was one of the deleted ones, so it was not in the live sitemap either and nothing tripped. Re-run the control with a page you have confirmed is in the live sitemap.

## The two scripts

Since 2 September 2026 a hand deploy is one of two repo scripts, never a bare wrangler call, because the bare call skips everything the dead workflows used to do:

- `scripts/deploy-live.sh` runs `predeploy-check` and then deploys `dist` to the production branch.
- `scripts/deploy-staging.sh` reproduces `deploy-staging.yml`: the gate's own tests, then a throwaway directory holding a copy of `dist` with `X-Robots-Tag: noindex` appended, a Disallow-all `robots.txt`, no sitemaps, and `functions/_middleware.js` set to the password gate, deployed to the `staging` branch. The repo is never modified and the live site's own `functions/` (the pages.dev redirect) is never what staging gets.

The staging one exists because staging was found answering **200 to anyone**, serving the live `robots.txt`, with no gate: a hand deploy had shipped `dist` alone. Test a gated staging by its status code, `curl -sI` and expect 401; a 200 means the gate is not there.

Both scripts take the token from the environment (`CLOUDFLARE_API_TOKEN`, `CLOUDFLARE_ACCOUNT_ID`) and never from a file.

## The standing rule

**Merge to `main` first, build from `main`, deploy `main`.** With Actions dead the deploy is manual, which is exactly why the branch discipline has to be.

More than one session writes articles at once, so before deploying, check for unmerged article work on other branches:

```
git fetch origin --prune
for b in $(git branch -r | sed 's/^ *//' | grep -v HEAD); do
  comm -23 <(git ls-tree -r --name-only "$b" src/content/articles | sort) \
           <(git ls-tree -r --name-only origin/main src/content/articles | sort)
done
```

Sort branches by last commit date and look at anything touched in the last few days. Leave out another session's in-flight work — a piece dated in the future and never live is being written right now, not waiting for you to ship it.

## Restoring, when it has already happened

Nothing is lost; the pages are in git. Merge `origin/main` and every branch holding unmerged articles into one tree, build, run the gate, deploy. Then verify **at the origin**, not through the cache: check every restored slug with the `?cb=` request above, confirm the live sitemap URL count went up by the number restored, and confirm the homepage and RSS carry them.

Count sitemap URLs with `grep -o '<loc>' | wc -l`. The XML is a single line, so `grep -c` returns 1 however many URLs it holds — which reads as a broken sitemap and starts a second false hunt.

Drafts are not losses. A `draft: true` article never had a live page, so its 404 after a restore is correct.

## Related

- [[Edge Security Lives at the Cloudflare Zone (YFarmX)]] — the other half of what the repo cannot see
- [[Staging and Backups (YFarmX)]] — the review flow the hand deploys bypass
- [[Pipeline Security Rules (YFarmX)]]
- [[YFarmX]]
- [[Map - Processes]]

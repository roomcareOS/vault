---
tags: [process, yfarmx, images, sourcing]
source: Falcon 9 reuse project build, 21 August 2026
updated: 2026-08-21
---

# Sourcing photographs from Wikimedia Commons (YFarmX)

**The rule: documentary pages carry real photographs with checked licences;
concept art explains hardware and is labelled as such (decision 75). Commons
is the store for real photographs; this is the pipeline that works from a
Claude session container.**

## The pattern that works (and the two that 429/503)

1. **Search and verify through the API, with a descriptive User-Agent.**
   Anonymous requests get 429'd. This works:

   ```
   UA="YFarmXNewsroom/1.0 (https://yfarmx.com; contact via yfarmx.com) curl"
   curl -s -A "$UA" "https://commons.wikimedia.org/w/api.php?action=query&format=json&prop=imageinfo&iiprop=url|size|extmetadata&iiurlwidth=1440&titles=File:<title>"
   ```

   Read `extmetadata.LicenseShortName` and `Artist` from the response. Only
   CC0, Public domain and CC BY ship; reject CC BY-SA and anything unclear.
   The licence goes in the on-page credit chip ("SpaceX · CC0",
   "NASA · public domain").

2. **Download the 1920px THUMBNAIL, never the original.** The bare
   `upload.wikimedia.org/wikipedia/commons/<hash>/<file>` originals return
   the rate-limit page even with the UA. The cached thumb bucket works, with
   UA plus Referer:

   ```
   curl -s -A "$UA" -H "Referer: https://commons.wikimedia.org/" \
     "https://upload.wikimedia.org/wikipedia/commons/thumb/<hash>/<file>/1920px-<file>"
   ```

   Thumb sizes are bucketed; ask the API (`iiurlwidth`) for a valid URL
   rather than guessing a width. Space downloads ~1.5 s apart.

3. **Resize with sharp into the page's own variants** (720/1440 webp,
   quality 80/82). A file only available small (the DSCOVR grid-fin webcast
   grab is 502px) can ship upscaled when the authentic frame beats a sharper
   substitute; caption it honestly.

## Fact-check the photo like a claim

The Commons record is a primary source for the photograph itself: EXIF date,
photographer, description. The Crew-5 port-return date (8 October 2022)
could not be confirmed from news coverage but is stated in NASA's own photo
record (Bill Ingalls, "towed into Port Canaveral... Saturday, Oct. 8,
2022"). Check `extmetadata` before printing a date the copy hangs on.

## Worked example

The Falcon 9 reuse project page (`/space/hardware/falcon-9-reuse/`,
decision 74): nine timeline photographs, one hero, one anatomy frame, one
full-bleed break, all sourced this way in one session, licences named on
every credit. Scraper agents searched and verified; the main session
downloaded on the pattern above (`fetch-photos.mjs` in the session
scratchpad shows the shape: plan JSON in, sized variants out).

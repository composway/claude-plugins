# How these feeds lie about a zero

Read this before reporting a decline, a gap, or a lost keyword. Every trap here has
the same shape: **an absent row is not a measured zero**, and treating it as one
manufactures a finding that isn't there.

Each tool declares the ones that apply to the window you asked for, in the
response's `unsupportedInputs`. That declaration is per request, not per tool — a
site under the ceiling is not warned about a problem it doesn't have. Read it;
don't assume from this page.

## Google's row ceiling (~5,000 rows/day)

Google stores only about 5,000 rows per day per single-dimension breakdown once a
day is more than roughly three days old. Above that, its feeds are top-N
leaderboards exactly like Bing's.

- A missing row means **"not in the top ~5,000"**, never "zero".
- It is unrecoverable — a backfill cannot get the rows back, because they no longer
  exist on Google's side.
- The long tail is what gets cut, so a truncated window **overstates the branded
  share** of traffic.
- `get_site_snapshot`, `get_performance_trend`, `get_page_queries` and
  `get_query_pages` are unaffected and stay authoritative.

## A Google zero and a Bing null mean different things

In the period series behind `find_content_decay`, `find_emerging_keywords` and
`diagnose_traffic_losses`:

- A **Google** period with no row arrives as **`0`**.
- A **Bing** period with no row arrives as **`null`**, because Bing's feeds are
  top-N cursors and an absence there is not a fall.

Combine that with the row ceiling and you have the main false-decline generator: on
a truncated Google period, that `0` is a *missing row*, and a run of them reads as
exactly the decline you're looking for. Check the truncation declaration before
believing a `sustained` shape.

## Position is not comparable across engines

- `get_site_snapshot`'s `total.avgPosition` is **always null** by design — the two
  engines' position scales are not confirmed comparable, so a blended figure would
  not be a real number on either.
- There is no cross-engine rank-bucket total either: distinct-query counts are not
  additive across engines.
- `position` from `get_advanced_search_analytics` is null unless you group by
  `searchEngine`.

Report per engine, or don't report position.

## Dimensions neither engine can answer

- **No country or device breakdown**, on either engine, on any tool. Bing exposes
  none on any traffic endpoint. `get_advanced_search_analytics` rejects them *by
  name* with the reason — don't retry with a synonym.
- **`searchVertical` is Google-only.** Anything other than `web` returns Google
  alone, with Bing excluded and declared. `find_cross_engine_gaps` with no second
  engine returns no rows at all.
- The CTR benchmark behind `find_ctr_opportunities` is **site-wide only** — there is
  no per-country or per-device curve.

## The page×query join under-counts page totals

`get_page_queries` and `get_query_pages` read a page×query join, which under-counts
a page's own totals on both engines: Google drops anonymized queries from any
grouping that names `query`, and Bing's rows are fanned out from its top-pages
cursor on weekly snapshot dates.

Use these tools to **compare queries with each other**. Take a page's real totals
from `get_performance_trend`.

## Short history is usually Google's limit, not a bug

Search Console data begins at **property verification** and retention tops out
around 16 months. A site whose history looks short has almost always just been
verified recently — say that, rather than reporting a data problem or proposing a
backfill that cannot exist.

## "This query is absent" is never evidence

Both engines list a query only if the site already drew an impression for it. A
topic you have no page for produces **no row in any feed**.

This is why `find_content_gaps` has a second half, and why an empty `uncovered`
needs `uncoveredStatus` read before you conclude anything: `not_requested`,
`unavailable` (no confirmed products or audiences to imply topics from) and
`failed` all produce the same empty array, and none of them means the site has no
uncovered demand.

## Cannibalization is invisible to the search data alone

Both engines show one result per site, so the page that loses every time draws no
impressions and appears in no feed — and that settled case is the damaging one.
This is why `detect_keyword_cannibalization` reads the sitemap inventory too, and
why its `unknown` verdict (no usable inventory) must never be reported as
"cleared".

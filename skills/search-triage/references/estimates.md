# Reading `estimatedClicks`

`score_seo_opportunities` prices every row in one unit: `estimatedClicks`, the
clicks **per `windowDays`** at stake if the finding were fixed. That single unit is
what makes the queue rankable, and it is also the easiest number on this surface to
misreport.

## Never add `signals` together

The score takes the **maximum** of overlapping signals on one entity, never the
sum. A query that is both low-hanging and under-clicking is *one* recovery, not
two.

So: report the row's `estimatedClicks`. Do not add up the `signals` array, and do
not add two rows that name the same entity — see the `entityType` note below.

## Check `basis` before you promise a recovery

`basis` says what the number assumes, and the response's `estimateBases` spells
each one out. The distinction that matters:

- **`decay` rows** are clicks that were *measurably earned and then lost*. The
  strongest claim available — this traffic demonstrably existed.
- **Every other basis** is a *projection* onto the site's own CTR curve: what the
  site's own page-one rows suggest this entity would earn if promoted. A
  projection, not a measurement.

Phrase them differently. "Recovering ~120 clicks/month that this page used to
earn" is a fair claim for a decay row. For a projection, "if it reached page one,
the site's own CTR curve suggests ~120 clicks/month" is the honest form.

## A null `estimatedClicks` is not zero

`null` means the site's data could not price that row — usually too little
page-one data of its own to project from. Those rows sort last, but they are not
worthless, and reporting them as "0 clicks at stake" is wrong. Say the row could
not be priced.

The same applies to `estimatedUpliftClicks` on `find_low_hanging_keywords`.

## Queries and pages are different rows

The queue mixes `entityType` values because the four sources work on different
objects. **The same string as a query and as a page is two rows.** Don't dedupe
them by name, and don't sum them.

## The queue is a top-N of what was visible

`sources` reports which finding types were evaluated, which were excluded, and how
many candidates each produced. Report that rather than implying the queue is
exhaustive.

This matters most under Google's row ceiling (see
[`data-traps.md`](data-traps.md)): a *ranked queue* reads as "these are the best
available actions" in a way a filtered list does not. When the response declares a
truncated window, say the queue is a top-N of **what was visible**, not of the
site.

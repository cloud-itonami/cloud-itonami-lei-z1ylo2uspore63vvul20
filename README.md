# cloud-itonami-lei-z1ylo2uspore63vvul20

> **Disclaimer**: This is an independent third-party archive/analysis. Not affiliated with, endorsed by, or sponsored by Discover Financial Services.

Archives the publicly published Terms of Service / legal document for **Discover Financial Services**, keyed by its ISO 17442 Legal Entity Identifier (LEI), per [ADR-2607110300](https://github.com/com-junkawasaki/root/blob/main/90-docs/adr/2607110300-cloud-itonami-lei-corporate-tos-catalog.md) (cloud-itonami-lei-corporate-tos-catalog).

## Company

- **Legal name**: Discover Financial Services
- **LEI**: `Z1YLO2USPORE63VVUL20`
- **LEI record**: https://api.gleif.org/api/v1/lei-records/Z1YLO2USPORE63VVUL20
- **Jurisdiction**: United States (Delaware) — GLEIF `US-DE`
- **Website**: https://www.discover.com
- **Ticker**: DFS (NYSE)

## Files

| Path | What it is |
| --- | --- |
| `blueprint.edn` | The identity this repository is keyed by. Hand-maintained. |
| `facts.edn` | 9 verified registry facts with per-fact provenance. **Generated** — see below. |
| `80-data/public/tos.journal.edn` | The archived legal document, as an `[e a v tx op]` journal. |
| `80-data/public/site.journal.edn` | Official-website enrichment (title, description, reachability). |
| `scripts/verify-facts.cljs` | Re-fetches every source `facts.edn` cites and fails if the live record disagrees. |

## Verifying the record

The LEI fields above used to be assertions with nothing in the repository behind
them. `facts.edn` now carries them as data, and every value in it was read out of
a public registry response whose URL and retrieval time sit next to the value:

```
nbb scripts/verify-facts.cljs           # check the recorded facts against the live sources
nbb scripts/verify-facts.cljs --write   # re-fetch and rewrite facts.edn
```

Nine GLEIF endpoints back the file — the LEI record, its ISINs, its managing LOU,
that LOU's accreditation as an LEI issuer, the direct- and ultimate-parent
reporting exceptions, its direct-children page, registration authority `RA000602`
(Delaware Division of Corporations), and ISO 20275 legal form `XTIQ`
(Corporation). All nine answered `200` when the file was written.

The checker exits **0** when every citation resolves and every recorded fact
still matches, **1** when a citation breaks or a fact drifts, and **3** when it
could not perform the check at all — an absent or unreadable `facts.edn`, or
every request failing at the transport level. A check that could not run must not
be indistinguishable from a check that ran and found nothing, so it refuses to
report a pass rather than exiting 0.

### Two numbers are counts, not lists

`:securities/isin-count` (851) and `:relationship/direct-child-count` (0) are read
from `meta.pagination.total` of a page the script actually fetched. The 851
individual ISINs are deliberately **not** mirrored here: at this issuer's volume
they turn over as instruments mature and are issued, which would make the checker
red for reasons that are not "the citation broke", and a gate that is always red
is a gate nobody reads. Walk the cited `/isins` URL's 57 pages to enumerate them.

The zero is a **measured** zero — GLEIF lists no directly consolidated children —
and it is recorded as its own entity precisely so that "no children" and "nobody
asked" do not look the same in this file. If a child appears, the checker reports
it as `ADDED` and exits 1.

### The LEI registration is lapsed

Two status fields in `facts.edn` are different things and disagree:

- `:company/status` `"ACTIVE"` — the entity, per GLEIF
- `:registration/status` `"LAPSED"`, `:registration/conformity-flag` `"NON_CONFORMING"`,
  `:registration/next-renewal-date` `"2025-12-29T19:03:00Z"`, last updated
  `2025-12-30`

An active company can hold a lapsed LEI registration. Both values are recorded as
the registry reports them; neither is corrected here. GLEIF records no successor
entity and no parent of either accounting-consolidation category
(`NON_CONSOLIDATING` for both).

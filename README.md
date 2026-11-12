# exo-search

Indexed full-text search over a personal Twitter archive — and the rest of a
personal corpus — in one local SQLite FTS5 index.

Built because per-question bespoke re-scans of a 53 MB `tweets.json` keep
producing stale, wrong answers. One persistent index, BM25-ranked queries,
exact term censuses, and a follow graph you can actually ask questions of.

## What it indexes

From a standard Twitter/X archive export (`~/workspace/twitter-archive/`):

- **tweets** — all of them (29,539 in the reference archive, 2008–2025; RTs flagged)
- **dms** — every direct message, 1:1 and group (44,822; sender and conversation ids carried on every hit)
- **likes** — every liked tweet's full text (47,973)
- **people** — the follow graph as a table, not prose: 2,575 accounts,
  1,773 followers, 1,699 following, 897 mutuals, with screen names resolved
  from mention data wherever the archive itself provides them

Plus the surrounding corpus, when present:

- **memory** — `~/MEMORY.md`, `~/USER.md`, `~/memory/**/*.md`, chunked with line anchors
- **phext** — `~/workspace/phext/*.phext`, split into scrolls by coordinate

Counts above are the reference build, each validated against an independent
brute-force count before the index was trusted (mutuals: 897 by graph table,
897 by set intersection).

## Usage

```
exo-search build [--quiet]                  # (re)build ~/workspace/search-index/exo.db
exo-search stats                            # per-source doc counts + people summary
exo-search query '<fts5 expr>' [--source tweets|dms|likes|memory|phext] [--after YYYY-MM-DD] [--before YYYY-MM-DD] [--limit N] [--json]
exo-search count '<term or phrase>' [--source X]   # exact doc census via the index
exo-search people '<handle-or-account-id>'  # follower? following? mutual? (snapshot-dated)
```

FTS5 expressions work as you'd expect: `phext`, `"dear internet"`,
`wish OR dream`, `internet NEAR dear`.

`people` prints the archive snapshot date (the reference export ends
2025-06-16) on every answer. The graph records what the export knew, not
what X knows today — treat it accordingly.

## Privacy

The index is a local file. This repository contains the tool only — no
archive data, no index, no message text is published here or anywhere else.
Name resolution tops out where the archive stops: accounts that were never
@-mentioned stay searchable by numeric id (999 of 2,575 resolved in the
reference build).

## Why this exists

Most software in this space competes on the software-factory game: another
app, another feed, another dashboard, scored on output volume. exo-search
is the opposite move — it is substrate. It makes one person's own record
*addressable*: every message, like, and relationship retrievable in a
second, with receipts, so the layer above it (judgment, connection,
decisions about who to reach and why) stops running on stale memory.

Build the substrate, not the game.

## License

MIT (see LICENSE).

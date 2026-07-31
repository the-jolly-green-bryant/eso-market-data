# Partitioned observation segments

This directory is the canonical observation store. Records remain JSON Lines, but
are grouped into deterministic gzip-compressed (`.jsonl.gz`) segments instead of
creating one file for every item variant and date.

Segments are partitioned by server, year, month, and a stable two-digit item shard:

```text
observations/<server>/<year>/<month>/<shard>.jsonl.gz
```

Each line contains the item identity, trait and quality dimensions, server, and one
point-in-time statistics record. Writers decompress only the affected segment,
deduplicate on that complete identity and date, sort records deterministically,
write deterministic gzip bytes atomically, and maintain
`data/manifests/observations.json` with counts, byte sizes, and SHA-256 checksums.

Website-facing histories are stored separately as one
`data/items/<shard>/<item-id>.pricing.zip` archive per item. Each archive retains
the former historical filename as its ZIP entry name so variant and platform
lookups remain targeted without hundreds of thousands of loose Git objects.

To convert a checkout that still has loose histories or uncompressed segments:

```bash
pnpm --filter @eso-market-tracker/data compress
```

The migration verifies every new archive before removing its source files.

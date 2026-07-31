# ESO Market Data

This repository is the versioned flat-file data source for
[ESO Market Tracker](https://esomarkettracker.com).

Application code lives in
[`eso-market-tracker`](https://github.com/the-jolly-green-bryant/eso-market-tracker).
Keeping the generated market snapshot here prevents ordinary website, API,
Discord, and utility changes from rebuilding or recommitting hundreds of
megabytes of data.

## Layout

- `items/` contains sharded item metadata and compressed pricing histories.
- `segments/` contains compressed observation segments.
- `images/` contains locally mirrored item imagery.
- `index/` contains the indexes consumed by the website and builders.
- `manifests/` describes the current observation and upstream versions.
- `addon/` contains derived data files for the in-game add-on.
- `snapshot.json` identifies the source application revision for this snapshot.

Large generated deployment artifacts such as SQLite databases are published as
GitHub release assets instead of being added to Git history.

The application repository consumes this repository as a shallow submodule.
Daily ingest commits only changed data here.


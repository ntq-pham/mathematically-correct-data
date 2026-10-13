# Mathematically Correct community data

Daily community card stats for
[Mathematically Correct](https://github.com/ntq-pham/mathematically-correct),
a decision-advisor mod for Slay the Spire 2. The mod downloads the table for
the player's game build from the `community` release of this repo.

This repo is written by a scheduled job in the mod's repo
(`.github/workflows/community-daily.yml`, running `pipeline/mc/community_daily.py`).
Do not edit it by hand: the job rewrites this README and replaces the release
assets.

## Source

The numbers are computed from the community run export of
[Spire Codex](https://spire-codex.com) ([GitHub](https://github.com/ptrlrd/spire-codex)),
one export per game build. Thanks to the Spire Codex maintainers and to
everyone who uploads runs there. Community data: spire-codex.com.

Only base-game runs at Ascension 10 are counted: standard, finished,
single-player runs played at Ascension 10 whose characters, acts, cards,
relics, potions, elites and bosses are all in that build's own catalog.
Modded runs and runs at any other ascension are left out.

## What the files are

The files are derived aggregates. They hold no run data: no runs, seeds,
player names, Steam IDs or upload tokens. Each table stores counts per card,
character and act phase (times offered, times picked, wins when picked, wins
when skipped), with rows below the minimum sample left out. Every run is A10,
so the tables (format 2) store one ascension band and record the scope in
their header.
Game data and names belong to Mega Crit.

| Asset | What |
|---|---|
| `index.tsv` | One row per build with a table |
| `community-<build>.mcstats` | The table for one build, for example `community-v0.111.0.mcstats` |

`index.tsv` is tab-separated, UTF-8, with LF line endings and one header line,
sorted by build. Its columns:

| Column | Meaning |
|---|---|
| `build` | The game build, for example `v0.111.0`. Release candidates look like `v0.112.0-rc.1` |
| `format_version` | The `.mcstats` format version (`CommunityStats.FormatVersion` in the mod) |
| `runs` | Base-game A10 runs counted |
| `codex_generated` | When Spire Codex generated the export manifest the table was built from |
| `export_sha256` | The sha256 of the Spire Codex export the table was built from |
| `table_sha256` | The sha256 of the `.mcstats` file |
| `table_bytes` | The size of the `.mcstats` file in bytes |

A table is rebuilt only when its build's export changes on Spire Codex.

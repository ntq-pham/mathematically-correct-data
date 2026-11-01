# Mathematically Correct community data

Daily community card stats and per-build win models for
[Mathematically Correct](https://github.com/ntq-pham/mathematically-correct),
a decision-advisor mod for Slay the Spire 2. The mod downloads the table and
the model for the player's game build from the `community` release of this
repo.

This repo is written by two scheduled jobs in the mod's repo:
`.github/workflows/community-daily.yml` (`pipeline/mc/community_daily.py`)
writes the stats tables, and `.github/workflows/model-daily.yml`
(`pipeline/mc/model_daily.py`) writes the models. Do not edit it by hand: the
jobs rewrite this README and replace the release assets.

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
so the tables store one ascension band and record the scope in their header.
Since format 3 a table also carries the numbers behind the mod's card page:
per card, the same counts by act phase and by copies already held, and the
mod's model averaged over the rewards that offered the card (which held cards
and relics it does better or worse with, and which deck shortfalls it fixes).
Format 2 tables have no card page numbers.
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

## The models

A model is trained per game build from the same export, on base-game A10 runs
only, and published only when it passes every quality gate of the mod's
`docs/MODEL.md` section 7. Its report says how it was checked and how it
differs from the model players had before. A build whose model fails a gate
gets no model; the mod keeps using the newest one it has, or the one it ships
with. The model files hold decision trees and the card, relic and encounter
tables they read. They hold no run data.

| Asset | What |
|---|---|
| `models.tsv` | One row per build with a model |
| `model-<build>.mcmodel` | The model for one build |
| `model-report-<build>.md` | How that model was trained and checked |
| `model-attempts.tsv` | Builds whose last training failed a gate, and which gates |

`models.tsv` has the same format as `index.tsv`, sorted by build version. Its
columns:

| Column | Meaning |
|---|---|
| `build` | The game build |
| `model_format` | The `.mcmodel` format version |
| `export_sha256` | The sha256 of the Spire Codex export the model was trained on |
| `model_sha256` | The sha256 of the `.mcmodel` file |
| `model_bytes` | The size of the `.mcmodel` file in bytes |
| `runs_train`, `runs_valid`, `runs_test` | The runs in each split of the training data |

A build is retrained only when its export changes after a failed attempt.

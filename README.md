# deep-dive-dwh 🐟

A personal **data engineering learning project**: a tracker and helper for the game *Dave the Diver*, built the way a real data warehouse is built.

> 🚧 Work in progress — this is a learning-in-public project. Expect rough edges.

## What it does (goal)

Dave the Diver is a complex game: fish in different zones, depths and times of day, a sushi restaurant with recipes that level up, and lots of crafting. This project answers questions like:

- Which fish am I still missing — and where and when can I catch them?
- Which ingredients do I need to cook a recipe *n* more times?
- Which items am I missing for a specific upgrade?

## Architecture

**Medallion architecture** with a dimensional (star schema) model in the gold layer.

| Layer | What happens |
|---|---|
| **Bronze** | Raw data from the Fandom wiki, loaded untouched |
| **Silver** | Cleaned, typed, deduplicated, tested |
| **Gold** | Star schema: dimensions (fish, location, time, recipe, …) and facts (fish availability, recipe ingredients, …) |

Two sources:
1. **Game data** from the [Dave the Diver Fandom wiki](https://davethediver.fandom.com/) via the MediaWiki API
2. **My own progress** (caught fish, cooked recipes, …) as dbt seeds

## Tech stack

- **Python** — extraction / ingestion
- **DuckDB** — local analytical database
- **dbt-core** with **dbt-duckdb** — transformations, tests, documentation
- **uv** — Python version, virtual environment and dependency management

## Getting started

Requires [uv](https://docs.astral.sh/uv/).

```bash
uv sync            # installs Python 3.12 + all dependencies into .venv
mkdir data         # local folder for the DuckDB file (not in Git)
cd dbt
uv run dbt debug   # checks the project and the DuckDB connection
uv run dbt run     # builds the models
```

## Roadmap

- [x] Setup
- [ ] Ingestion → Bronze (fish + locations)
- [ ] Silver: cleaning & tests
- [ ] Gold: star schema for fish availability
- [ ] dbt docs & lineage
- [ ] Own progress as a second source
- [ ] Recipes & ingredients
- [ ] Crafting & items

Architecture decisions are documented in [`docs/DECISIONS.md`](docs/DECISIONS.md).

## Data & license

Code: MIT.
Game data comes from the Dave the Diver Fandom wiki and is licensed under [CC BY-SA](https://www.fandom.com/licensing). Wiki data is **not** stored in this repository — it is fetched locally when the pipeline runs.

*Dave the Diver* is a game by MINTROCKET. This is an unofficial fan project.

# Architecture Decision Log

Key decisions for this project, with context and reasoning. Newest on top.

---

## ADR-005 — uv for Python and environment management

**Date:** 2026-10-08 · **Status:** Accepted

**Context:** The project needs Python (for ingestion and dbt), and the development machine had no Python installation yet. Dependencies should live in an isolated environment per project, so they never conflict with other projects. Two ways to get there: the classic setup (install Python manually, then `venv` + pip) or `uv`.

**Decision:** Use **uv** to install Python, create the virtual environment and manage dependencies.

**Consequences:** One tool covers the Python version, the environment and the packages — no separate Python install needed. Exact dependency versions are locked automatically (`uv.lock`), so the setup is reproducible. uv is increasingly common in modern data teams. Downside: most tutorials still show `venv` + pip, so commands sometimes need translating (e.g. `pip install` → `uv add`).

**Alternatives considered:** Python installer + `venv` + pip — the well-documented standard, but it requires installing Python separately and pinning dependencies by hand. Both options isolate dependencies per project equally well.

---

## ADR-004 — Own progress as dbt seeds

**Date:** 2026-10-07 · **Status:** Accepted

**Context:** The tracker needs a second data source: which fish I have caught, which recipes I have cooked or mastered. This data does not exist anywhere digitally — it is entered by hand.

**Decision:** Start with CSV files loaded as **dbt seeds**.

**Consequences:** Simple and version-controllable. Not a great input experience, but good enough to learn how to join manual data to a dimensional model. A nicer input method can come later.

---

## ADR-003 — MediaWiki API with local raw cache

**Date:** 2026-10-07 · **Status:** Accepted

**Context:** Game data comes from the Fandom wiki. Options: scraping rendered HTML or using the MediaWiki API.

**Decision:** Use the **MediaWiki API** where possible. Store raw responses locally and load them untouched into the bronze layer.

**Consequences:** Less fragile than HTML scraping, gentler on the wiki (fewer requests), and pipeline runs are reproducible without hitting the source again. Raw data is not committed to Git (license and size).

---

## ADR-002 — Python for ingestion

**Date:** 2026-10-07 · **Status:** Accepted

**Context:** Extraction could be written in Python or Node.js.

**Decision:** **Python.**

**Consequences:** Fits the dbt ecosystem and the wider data engineering tooling.

**Alternatives considered:** Node.js.

---

## ADR-001 — Medallion architecture with DuckDB and dbt

**Date:** 2026-10-07 · **Status:** Accepted

**Context:** The goal is to build this project the way production data warehouses are built, at a small scale.

**Decision:** **Medallion architecture** (bronze / silver / gold) with a **star schema** in gold. **DuckDB** as the database, **dbt-core** for transformations, tests and documentation.

**Consequences:** No server needed — everything runs locally as a single file. dbt brings testing, lineage and documentation out of the box.

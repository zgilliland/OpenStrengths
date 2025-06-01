# OpenStrengths
*An open‑source, community‑driven framework for mapping human potential.*

## Why OpenStrengths?

Most talent assessments lock their items, scoring, and data behind paywalls.  
OpenStrengths flips that model on its head:

* **Open science** – every item, algorithm, and dataset is public.  
* **Extensible** – add new strengths or localize to any language via simple YAML/CSV.  
* **AI‑ready** – clean schemas for adaptive testing, chatbots, and HR‑tech APIs.  
* **Apache‑2.0** – free for personal and commercial use.

---

## Quick Start

```bash
# clone the repo
git clone https://github.com/YOUR-ORG/openstrengths.git
cd openstrengths

# peek at the trait taxonomy
cat model/traits.yml

# score a sample response file (replace with your own CSV)
python scoring/score.py responses_sample.csv

## Directory Layout
```bash
├── LICENSE               # Apache-2.0
├── README.md             # this file
├── CONTRIBUTING.md       # how to propose changes
├── model/
│   └── traits.yml        # 6 domains × 36 facets
├── items/
│   └── items_v0.csv      # seed item bank (36 items)
├── scoring/
│   └── score.py          # simple mean → z → rank scorer
├── data/                 # (empty) anonymized pilot datasets
└── docs/                 # background research & design docs
```
## Understanding items/items_v0.csv
| Column           | Meaning                                                      | Example                                       |
| ---------------- | ------------------------------------------------------------ | --------------------------------------------- |
| `item_id`        | Unique question ID                                           | `Q007`                                        |
| `stem`           | Statement participants rate (Likert 1-5)                     | *I regularly demonstrate resilience at work.* |
| `domain`         | One of the six high-level buckets                            | Drive                                         |
| `facet`          | Specific strength the item measures                          | Resilience                                    |
| `keyed_positive` | `TRUE` if agreement → higher score; `FALSE` if reverse-keyed | `TRUE`                                        |

Why only 36 rows?
Each facet currently has one seed item. This illustrates the schema; the community will expand to 4–6 items per facet (~200 total) before Calibration-1.

## How Scoring Works (for now)
1. `score.py` reads a response CSV (`respondent_id,Q001,Q002,…`).

2. Reverse-scores any item where `keyed_positive == FALSE`.

3. Averages responses by facet, then ranks the 36 facets.

> Roadmap: Replace this simple scorer with a Bayesian 2-PL IRT engine once ≥ 2 000 pilot responses are collected.

## Contributing
We ❤️ pull requests! Good first tasks:

| Task                | How-to                                                                                         |
| ------------------- | ---------------------------------------------------------------------------------------------- |
| **Add an item**     | Fork → append a new row to `items/items_v0.csv` (use next `Q###`)                              |
| **Improve wording** | Suggest clearer, culture-neutral language for existing items                                   |
| **Pilot test**      | Survey your cohort, anonymize the CSV, drop it into `/data/`, open a PR with reliability stats |
| **Code**            | Help wire reaction-time capture, adaptive CAT, or the IRT scorer                               |


See **CONTRIBUTING.md** for the IP checklist and psychometric-evidence requirements.

## Roadmap
| Milestone                | Target                        | ETA     |
| ------------------------ | ----------------------------- | ------- |
| **Item Bank v1**         | ≥ 200 items (4–6 per facet)   | 2025 Q3 |
| **Calibration-1**        | 2-PL IRT parameters published | 2025 Q4 |
| **Forced-Choice Engine** | Fake-detection AUC ≥ 0.85     | 2026 Q1 |
| **Public API v1**        | JSON endpoints & auth         | 2026 Q2 |
| **Adaptive Test Beta**   | Avg completion ≤ 15 min       | 2026 Q4 |


## License
OpenStrengths is released under the Apache License 2.0 – free for personal and commercial use.
See LICENSE for full text.

> OpenStrengths is not affiliated with Gallup, Inc. “CliftonStrengths®” and related marks are trademarks of Gallup, Inc.


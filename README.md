# LakeQA benchmark website

Static website for **LakeQA: An Exploratory QA Benchmark over a Million-Scale Data Lake** (ICML 2026).

Vanilla HTML/CSS/JS — no framework, no build step. Designed to be served directly from GitHub Pages.

- Website: https://lakeqa-bench.github.io/
- Code: https://github.com/lakeagent/datalake-qa
- Data roots: `s3://lakeqa-yc4103-datalake/datagov`, `s3://lakeqa-yc4103-datalake/wikipedia`

## Layout

```
/
├── index.html              # Home: HotpotQA-style two-column (intro + pipeline + citation | leaderboard)
├── details.html            # Task definition, worked example, metrics, why it's hard
├── assets/
│   ├── css/style.css
│   ├── js/leaderboard.js
│   ├── papers/
│   │   └── lakeqa_arxiv.pdf # Public arXiv/camera-ready PDF linked from the home page
│   ├── task_7.json         # Source of truth for the worked example on details.html
│   └── data/
│       ├── leaderboard_full.json
│       └── leaderboard_mini.json
├── .nojekyll               # Tells GitHub Pages not to run Jekyll
└── README.md
```

## Run locally

```sh
python3 -m http.server 8000
```

Then open `http://localhost:8000/`.

## Deploy

Push to `main`. GitHub Pages serves the repo root. The `.nojekyll` file disables Jekyll so paths under `assets/` work as-is.

## Submitting an entry

The leaderboard lives on the home page (right column, with two tabs for the Full and Mini splits). To add an entry, open a pull request that:

1. Adds an object to `assets/data/leaderboard_full.json` and/or `assets/data/leaderboard_mini.json`.
2. Links a publicly hosted trace bundle for your run (separate repo, S3 bucket, HuggingFace dataset, etc.).
3. Optionally includes a short system description in `notes`.

### JSON schema

Each leaderboard entry is one object:

```json
{
  "rank": 1,
  "model": "Your System Name",
  "em": 23.08,
  "runtime_s": 124.45,
  "cost_usd": 0.96,
  "dacc_p": 34.18,
  "dacc_r": 33.27,
  "dacc_f1": 40.03,
  "dret_p": 2.61,
  "dret_r": 42.12,
  "dret_f1": 5.86,
  "reported": "2026-05",
  "source": "https://link-to-your-paper-or-traces",
  "notes": "Optional short note about your system"
}
```

| Field | Type | Notes |
|---|---|---|
| `rank` | int | Display rank — informational; the table re-sorts on column click. |
| `model` | string | System or model name. |
| `em` | float | Exact-match score (%). |
| `runtime_s` | float | Wall-clock seconds per task. |
| `cost_usd` | float | US dollars per task. |
| `dacc_p` / `dacc_r` / `dacc_f1` | float | Precision / recall / F1 (%) on the accessed-set `D_acc` vs the gold set `D*`. |
| `dret_p` / `dret_r` / `dret_f1` | float | Precision / recall / F1 (%) on the retrieval-set `D_ret` vs `D*`. |
| `reported` | string | YYYY-MM the result was reported. |
| `source` | string | Link to a paper, blog post, or traces repo. |
| `notes` | string | Optional, short. |

See the [Details page](details.html#metrics) for the metric definitions.

## Pre-launch checklist

Before pushing the repo public:

1. Confirm the homepage points to the current public code repository.
2. Confirm the only committed PDF is the public arXiv/camera-ready PDF at `assets/papers/lakeqa_arxiv.pdf`.
3. `grep -ri "confidential reviewer" .` must return no results inside committed source.

## License

See the public benchmark repository for code and data licensing details: <https://github.com/lakeagent/datalake-qa>.

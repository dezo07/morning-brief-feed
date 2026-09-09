# data/raw_news — Raw News Archive

This directory is the permanent archive of raw headlines collected each day for the morning brief pipeline. It also serves as the data source for the future chatbot.

## Directory structure

```
data/raw_news/
└── YYYY-MM-DD/
    ├── zerohedge.json      ← written by Windows pipeline (collector.py)
    ├── investing.json      ← written by Windows pipeline (collector.py)
    └── financialjuice.json ← pushed by Ubuntu auto-update script
```

Each subdirectory is named after the **brief date** (`YYYY-MM-DD`), created automatically when the pipeline runs.

## How each file is produced

| File | Producer | When |
|------|----------|------|
| `zerohedge.json` | `morning-brief/src/collector.py` | During the morning pipeline run on Windows |
| `investing.json` | `morning-brief/src/collector.py` | During the morning pipeline run on Windows |
| `financialjuice.json` | Ubuntu auto-update script | Overnight / early morning, committed and pushed before 06:00 EST |

## JSON format

Every file is a JSON array of article objects:

```json
[
  {
    "title": "Fed's Waller: Still Need More Data Before Considering Rate Cuts",
    "published": "2026-06-11T04:30:00+00:00",
    "source": "FinancialJuice"
  }
]
```

Required fields:
- `title` — headline text (string)
- `published` — ISO 8601 timestamp with timezone, ideally UTC (`+00:00`)
- `source` — source name string (e.g. `"FinancialJuice"`, `"Zerohedge"`, `"Investinglive"`)

## Important: path is at the repo root

The `data/raw_news/` directory lives at the **repo root** (`morning-brief-clone/data/raw_news/`), **not** inside the `morning-brief/` Python project subfolder.

The Python code resolves this path via:
```python
Path(__file__).resolve().parent.parent.parent / "data" / "raw_news"
```
so it works regardless of the working directory when `run.py` is invoked.

## Git policy

- `data/raw_news/` is **committed** — it is the archive. Do not add it to `.gitignore`.
- `data/audio/`, `data/logs/`, and `data/polymarket_cache/` remain gitignored.

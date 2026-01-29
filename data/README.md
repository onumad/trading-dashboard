# Trading Data Storage

Historical daily snapshots stored as JSON files.

## Format
Each file is named `YYYY-MM-DD.json` and contains:
```json
{
  "date": "2026-01-29",
  "dayNumber": 1,
  "totalPnl": 3529.00,
  "accounts": {
    "lucid": { ... },
    "apex1": { ... },
    ...
  }
}
```

## Usage
Run the update scripts with CSVs to generate daily snapshots.

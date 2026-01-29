# CSV Upload Guide

## Uploading Trades

The dashboard now supports uploading trades directly from Tradovate CSV exports!

### How to Upload

1. **Export from Tradovate**
   - Log into Tradovate
   - Go to your trade history/reports
   - Export as CSV

2. **Upload to Dashboard**
   - Click the green **📤 Upload Trades (CSV)** button at the top
   - Select your CSV file
   - Wait for "✅ X trades uploaded and synced!" notification

3. **Automatic Sync**
   - Trades are saved to Firebase
   - Instantly available on all your devices
   - Dashboard refreshes automatically

### CSV Format (Tradovate)

The expected format:

```
Contract,Buy/Sell,Qty,Avg. Entry Price,Avg. Exit Price,Realized P&L,Time Closed
GCJ6,Buy,1,5547.00,5548.50,145.40,2026-01-29 10:30:00
MGCJ6,Sell,2,5165.20,5139.30,515.32,2026-01-29 14:15:00
```

### Filename Convention (Optional)

Name your file with the date for automatic detection:

- `trades-2026-01-29.csv` → Uploads to Jan 29
- `2026-01-30.csv` → Uploads to Jan 30
- `my-trades.csv` → Uses currently selected date

### What Gets Stored in Firebase

```json
{
  "trades": {
    "2026-01-29": [
      {
        "contract": "GCJ6",
        "type": "LONG",
        "entry": 5547.00,
        "exit": 5548.50,
        "qty": 1,
        "pnl": 145.40,
        "timestamp": "2026-01-29 10:30:00"
      }
    ]
  }
}
```

### Firebase Rules

Make sure your Firebase Realtime Database rules allow trade uploads:

```json
{
  "rules": {
    "goals": {
      ".read": true,
      ".write": true
    },
    "trades": {
      ".read": true,
      ".write": true
    }
  }
}
```

### Troubleshooting

**"Firebase not configured" error:**
- Make sure your Firebase config is correct in `index.html`
- Check that Realtime Database is enabled in Firebase Console

**"Failed to parse CSV" error:**
- Verify your CSV has the correct headers
- Make sure there are no special characters or formatting issues
- Check that numbers don't have currency symbols ($)

**Upload button doesn't work:**
- Hard refresh the page (Ctrl+Shift+R)
- Check browser console for errors (F12)

### Data Priority

- **Firebase trades** (if available) take priority over JSON file trades
- This allows you to update trades without regenerating the daily snapshot
- JSON files remain as a backup/historical record

---

## Future Enhancements

Potential features:
- [ ] Bulk upload multiple days at once
- [ ] Edit individual trades via UI
- [ ] Delete trades
- [ ] Trade statistics/analytics
- [ ] Export trades back to CSV

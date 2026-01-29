# Firebase Setup Guide

## Step 1: Create Firebase Project

1. Go to [Firebase Console](https://console.firebase.google.com/)
2. Click "Add project"
3. Name it: `trading-dashboard` (or whatever you prefer)
4. Disable Google Analytics (optional, not needed)
5. Click "Create project"

## Step 2: Enable Realtime Database

1. In your Firebase project, click "Realtime Database" in the left menu
2. Click "Create Database"
3. Choose location: `United States (us-central1)` (or closest to you)
4. Start in **test mode** for now (we'll secure it later)
5. Click "Enable"

## Step 3: Get Configuration

1. Click the gear icon ⚙️ next to "Project Overview"
2. Click "Project settings"
3. Scroll down to "Your apps"
4. Click the web icon `</>`
5. Register app (name: "Trading Dashboard")
6. Copy the `firebaseConfig` object

## Step 4: Update Dashboard

Open `index.html` and replace this section:

```javascript
const firebaseConfig = {
    apiKey: "YOUR_API_KEY",
    authDomain: "YOUR_PROJECT_ID.firebaseapp.com",
    databaseURL: "https://YOUR_PROJECT_ID-default-rtdb.firebaseio.com",
    projectId: "YOUR_PROJECT_ID",
    storageBucket: "YOUR_PROJECT_ID.appspot.com",
    messagingSenderId: "YOUR_MESSAGING_SENDER_ID",
    appId: "YOUR_APP_ID"
};
```

With your actual config from Firebase.

## Step 5: Secure the Database (IMPORTANT!)

1. Go to "Realtime Database" → "Rules"
2. Replace the rules with:

```json
{
  "rules": {
    "goals": {
      ".read": true,
      ".write": true
    }
  }
}
```

**For better security (recommended):**

```json
{
  "rules": {
    "goals": {
      ".read": "auth != null",
      ".write": "auth != null"
    }
  }
}
```

Then enable Authentication → Email/Password in Firebase Console.

## Step 6: Test It

1. Commit and push changes to GitHub
2. Wait ~30 seconds for GitHub Pages to rebuild
3. Open dashboard in **two different browsers**
4. Click the ✏️ icon and set a custom goal
5. Check the other browser - it should update automatically! 🎉

## Troubleshooting

### "Firebase not configured" warning
- Check that you replaced ALL placeholder values in firebaseConfig
- Make sure databaseURL matches your Realtime Database URL

### Goals not syncing
- Open browser console (F12) and check for errors
- Verify database rules allow read/write
- Check Firebase Console → Realtime Database → Data tab to see if data is being saved

### Page won't load
- Check browser console for errors
- Verify Firebase CDN scripts are loading (check Network tab)

---

## Fallback Mode

If Firebase setup fails, the dashboard automatically falls back to localStorage (local-only storage). You'll see this warning in console:

```
⚠️ Firebase not configured, using localStorage fallback
```

This means goals will only save per-browser until Firebase is configured.

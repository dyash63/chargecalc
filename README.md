# Charge Calc

EV charging cost calculator – track **home** and **outside** charging sessions.

PWA with Google Sign-In, private Firestore data per user, and full offline support.

## Features

- Log home & outside charging sessions
- Input by **battery %** or **kWh**
- Cost calculation from energy and rate
- Edit & delete past sessions
- Monthly stats (spent, energy, sessions)
- History filters (location, month)
- Offline-first: works without network, syncs when back online
- Google Sign-In (private data per account)
- Installable on iPhone (Add to Home Screen)
- Dark mode, multi-currency

## File structure

```
chargecalc/
├── index.html              # App (React + Firebase)
├── manifest.json           # PWA manifest
├── sw.js                   # Service worker
├── firestore.rules         # Security rules (copy into Firebase Console)
├── README.md
└── icons/
    ├── apple-touch-icon.png
    ├── icon-180.png
    ├── icon-192.png
    └── icon-512.png
```

## Firebase setup (required once)

1. Open [Firebase Console](https://console.firebase.google.com) → project **chargecalc-eb2dd**

2. **Authentication**
   - Go to **Build → Authentication → Sign-in method**
   - Enable **Google**
   - Set support email if asked
   - Save

3. **Firestore**
   - Go to **Build → Firestore Database**
   - Create database if it doesn’t exist (start in production mode is fine)
   - Open **Rules** tab
   - Paste the contents of `firestore.rules` and **Publish**

4. **Authorized domains** (for Google Sign-In)
   - Authentication → Settings → Authorized domains
   - Add your hosting domain (e.g. `your-app.netlify.app` or `yourusername.github.io`)
   - `localhost` is already allowed for local testing

## Deploy

### Option A – GitHub + Netlify / Vercel / GitHub Pages

```bash
git push
```

Then connect the repo to Netlify, Vercel, or enable GitHub Pages.

### Option B – Firebase Hosting

```bash
npm install -g firebase-tools
firebase login
firebase init hosting   # select existing project chargecalc-eb2dd, public = .
firebase deploy
```

## Local testing

Serve over HTTPS or localhost (service workers need a secure context):

```bash
npx serve .
# open http://localhost:3000
```

## iPhone – Add to Home Screen

1. Deploy the site over **HTTPS**
2. Open the URL in **Safari**
3. Share → **Add to Home Screen**
4. Launch from the home screen (standalone mode)

## Data layout

```
users/{uid}/
  sessions/{sessionId}   # charging logs
  settings/user          # vehicle, tariff, dark mode, etc.
```

Each user only sees their own data (enforced by `firestore.rules`).

Offline data is also stored in `localStorage` and synced when the user is online and signed in.

## Notes

- Default home tariff: ₹8.5 / kWh (editable in Settings)
- Outside rate is required per session
- You can switch between % and kWh input modes when logging a charge
- Sessions can be edited after creation

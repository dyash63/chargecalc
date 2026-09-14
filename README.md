# Charge Calc

EV charging cost calculator – track **home** and **outside** charging sessions.

PWA with Google Sign-In and private Firestore data per user.

## Features

- Log home & outside charging sessions
- Cost calculation from battery % and rate
- Monthly stats (spent, energy, sessions)
- Google Sign-In (private data per account)
- Works as installed app on iPhone (Add to Home Screen)
- Dark mode, multi-currency

## File structure

```
chargecalc-pwa/
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

## Deploy (Git + hosting)

### Option A – GitHub + GitHub Pages / Netlify / Vercel

```bash
cd chargecalc-pwa
git init
git add .
git commit -m "Charge Calc PWA with Google Auth"
git branch -M main
git remote add origin https://github.com/YOUR_USERNAME/chargecalc.git
git push -u origin main
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

## Notes

- Default home tariff: ₹8.5 / kWh (editable in Settings)
- Outside rate is required per session
- Calculator logic is unchanged from the original app

# Team Cup 2026 — Live Scoring

A single-page golf scoring app for the Prague Expat vs Bromma Sweden Team Cup at Beroun Golf Club, 24 October 2026.

Everything is in one file: `index.html`. No build step, no framework, no npm install.

---

## What you are about to do

There are three separate things, in this order:

1. **GitHub** — a free place to keep the code online.
2. **Vercel** — takes the code from GitHub and puts it on a real web address.
3. **Firebase** — a free database so every scorer sees the same live scores.

Steps 1 and 2 take about 15 minutes. Step 3 takes about 10.

**You can do 1 and 2 and stop.** The site will work and look right, but each phone keeps its own separate scores. Step 3 is what makes it a shared scorecard, so do it before the event.

---

## Step 1 — Create a GitHub account

1. Go to **https://github.com** and click **Sign up**.
2. Enter your email, pick a password and a username, and verify the email they send you.
3. Choose the **Free** plan when asked.

---

## Step 2 — Put the code on GitHub

You do not need to install anything. This is all done in the browser.

1. Once signed in, click the **+** in the top-right corner, then **New repository**.
2. **Repository name**: `team-cup-2026` (any name is fine, no spaces).
3. Leave it set to **Public**. (Private also works with Vercel, but public is simpler.)
4. Do **not** tick "Add a README file" — you already have one.
5. Click **Create repository**.
6. On the next screen, click the link **uploading an existing file**.
7. Drag `index.html` and `README.md` into the browser window.
8. At the bottom, click **Commit changes**.

Your code is now on GitHub.

> **Tip:** to change anything later, click the file in GitHub, click the pencil icon, edit, then **Commit changes**. Vercel will update the live site by itself within a minute.

---

## Step 3 — Put it live with Vercel

1. Go to **https://vercel.com** and click **Sign Up**.
2. Choose **Continue with GitHub** and allow the permissions it asks for. This links the two accounts.
3. On your Vercel dashboard click **Add New…** then **Project**.
4. Find `team-cup-2026` in the list and click **Import**.
5. Leave every setting exactly as it is. Framework Preset will say **Other** — correct. There is no build command needed.
6. Click **Deploy**.
7. Wait about 30 seconds. You will get a live address like `https://team-cup-2026.vercel.app`.

That address works for anyone, on any phone, with no login. Share it freely.

> **Changing the address:** in Vercel, open the project → **Settings** → **Domains** to rename it or connect your own domain.

---

## Step 4 — Firebase, so all scorers share one scorecard

Without this, two phones = two separate scorecards. Free tier is far more than enough.

### 4a. Create the project

1. Go to **https://console.firebase.google.com** and sign in with a Google account.
2. Click **Create a project**.
3. Name it `team-cup-2026`. Turn **Google Analytics off** — you do not need it.
4. Click **Create project**, then **Continue**.

### 4b. Create the database

1. In the left menu click **Build** → **Firestore Database**.
2. Click **Create database**.
3. Choose a location near you — `eur3 (europe-west)` is a good pick.
4. Select **Start in test mode**. Click **Next**, then **Enable**.

> Test mode allows anyone to read and write for 30 days. That is fine for an event in October, but see **Security** at the bottom of this file before then.

### 4c. Get your config

1. Click the **gear icon** (top-left, next to Project Overview) → **Project settings**.
2. Scroll down to **Your apps**.
3. Click the **web icon**, which looks like `</>`.
4. App nickname: `scoring`. Do **not** tick Firebase Hosting. Click **Register app**.
5. You will see a block of code containing `const firebaseConfig = { ... }`.
6. Copy the values inside the curly braces.

### 4d. Paste it into the app

1. Go back to your GitHub repository and click **index.html**.
2. Click the **pencil icon** to edit.
3. Near the top you will find this:

```js
const FIREBASE_CONFIG = {
  apiKey: "",
  authDomain: "",
  projectId: "",
  storageBucket: "",
  messagingSenderId: "",
  appId: ""
};
```

4. Fill in each value from Firebase, keeping the quote marks. It should end up looking like:

```js
const FIREBASE_CONFIG = {
  apiKey: "AIzaSyB1c...",
  authDomain: "team-cup-2026.firebaseapp.com",
  projectId: "team-cup-2026",
  storageBucket: "team-cup-2026.appspot.com",
  messagingSenderId: "123456789012",
  appId: "1:123456789012:web:abc123def456"
};
```

5. Click **Commit changes**.
6. Vercel redeploys automatically. Wait a minute, then reload your site.

**How to tell it worked:** the orange "saved on this device only" warning disappears from the scoring tabs.

---

## Before the event — checklist

- [ ] **Stroke index.** Tap ⚙ and enter Beroun's real hole difficulty ranking, 1–9 for the front nine and 1–9 for the back nine. Until you do, an orange warning shows and handicap strokes will land on the wrong holes.
- [ ] **Singles strokes.** Currently every singles match is set to zero strokes. Enter the shots each player gets inside each match.
- [ ] **Starting holes.** If you are doing a shotgun start, set each match's "Starts on" so scorers enter their first hole in the first column.
- [ ] **Clear test scores.** Each match has a "Clear scores" button.

---

## How the app works

- **Scramble / Fourball / Singles tabs** — one card per match. Tap to open, enter gross strokes per hole. Status updates live (2 up, AS thru 4, 3&2).
- **Handicaps** — strokes are applied automatically to the hardest holes, based on the stroke index you enter. A gold dot marks a hole where that player gets a shot.
- **Leaderboard** — running points total, 1 point a win, ½ each for a half.
- **Photos** — shared photo wall. Images are shrunk in the browser before saving.
- **Info** — countdown, singles matchups with player photos, and the day's schedule.

---

## Security, before the event

Test mode expires after 30 days and then everything stops saving. To fix it, in Firebase go to **Firestore Database** → **Rules** and replace what is there with:

```
rules_version = '2';
service cloud.firestore {
  match /databases/{database}/documents {
    match /{document=**} {
      allow read, write: if true;
    }
  }
}
```

Click **Publish**.

Be aware what this means: anyone who has your web address can change the scores. For a private golf day that is usually an acceptable trade for not making everyone log in. If you would rather lock it down, the options are to add Firebase Authentication, or simply not share the link beyond the players.

---

## Troubleshooting

**The site shows a blank page.** Open the browser console (on desktop, right-click → Inspect → Console) and look for a red error. Most often a typo in the Firebase config — a missing quote or comma.

**Scores are not syncing between phones.** Check the orange warning is gone. If it is still there, the config is not filled in correctly. Also confirm Firestore was actually created in Step 4b.

**"Missing or insufficient permissions" in the console.** Your test-mode window expired. Apply the rules above.

**Photos will not upload.** In device-only mode the browser storage limit is about 5MB. Connect Firebase and this goes away.

**I edited the file and nothing changed.** Vercel needs a minute. Check the **Deployments** tab in your Vercel project — if a deployment failed it will say so there.

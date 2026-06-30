# 🏏 DBL 2026 Live Auction

A single-page live cricket auction tool with admin controls, real-time viewer mode, and free Firebase sync.
**No server. No backend code. No build step.** Deploys as a static site on GitHub Pages.

---

## 🚀 Deploy to GitHub Pages (5 minutes)

### Step 1 — Create the repository
1. Go to [github.com/new](https://github.com/new)
2. Repository name: `dbl-auction` (or any name)
3. Visibility: **Public**
4. Click **Create repository**

### Step 2 — Upload the files
**Option A — Web UI (easiest):**
1. In your new repo → **Add file → Upload files**
2. Drag in `index.html` and `.nojekyll` from this folder
3. Commit message: `Initial deploy`
4. Click **Commit changes**

**Option B — Git CLI:**
```bash
git init
git add index.html .nojekyll
git commit -m "Initial deploy"
git branch -M main
git remote add origin https://github.com/YOUR_USERNAME/dbl-auction.git
git push -u origin main
```

### Step 3 — Enable GitHub Pages
1. Repo → **Settings → Pages**
2. Source: **Deploy from a branch**
3. Branch: `main` / folder: `/ (root)`
4. **Save**

Your auction goes live within ~60 seconds at:
```
https://YOUR_USERNAME.github.io/dbl-auction/
```

---

## 🔴 Enable Live Sync (Firebase — free, no card required)

### Get your Firebase database
1. [console.firebase.google.com](https://console.firebase.google.com) → **Add project** (disable Analytics, doesn't matter what you name it)
2. Left sidebar → **Build → Realtime Database → Create Database**
3. Choose a region → **Start in test mode**
4. Copy the database URL shown (e.g. `https://dbl-auction-default-rtdb.firebaseio.com`)

### Connect in the app
1. Open your GitHub Pages URL
2. **Admin Login** (default password: `admin2026` — change it after deploying, see below)
3. Tap **⚡ Sync** in the header
4. Paste the Firebase URL → **Connect**
5. A **Viewer Share Link** appears — copy it

### Share with participants
Send the share link (looks like `https://you.github.io/dbl-auction/?view=1&fburl=...`) via WhatsApp, email, or a QR code. Anyone who opens it sees the live auction with zero setup — no login, no Firebase knowledge needed.

> ⚠️ Firebase test mode allows public reads for **30 days**. After that, go to Firebase Console → Realtime Database → **Rules** and set:
> ```json
> { "rules": { ".read": true, ".write": "auth != null" } }
> ```

---

## 🔑 Changing the admin password

The default password is `admin2026`, hardcoded as a fallback in the JS (`_adminPw` variable). To set your own:

1. Open `index.html` in a text editor
2. Find this line near the top of the `<script>` section:
   ```js
   var _adminPw='admin2026';
   ```
3. Change `'admin2026'` to your own password
4. Re-upload to GitHub (overwrite the file, commit)

Alternatively, the password is also checked against `localStorage.getItem(ADMIN_PW_KEY)` — if you want to set it per-deployment without editing code, you can set this in the browser console once:
```js
localStorage.setItem('auction_admin_pw', 'your-new-password');
```

---

## 📱 How it works

| Role | Access |
|---|---|
| **Admin** | Full control: add players/teams, run auction, assign/undo, push live updates |
| **Viewer** | Read-only: watches live bids, sees SOLD notifications, can't edit anything |

The admin's browser auto-pushes every change to Firebase (debounced ~1.8s). Viewers poll every 5 seconds and get a toast notification the moment a player is sold.

---

## 🔄 Updating the app

1. Edit `index.html` locally, or directly in GitHub's web editor (pencil icon on the file)
2. Commit the change
3. GitHub Pages redeploys automatically within ~60 seconds — no extra steps

---

## 🛠 Tech notes

- **Single file** — all HTML, CSS, and JS in one `index.html`, zero build step
- **Storage** — `localStorage` for offline/local persistence, Firebase Realtime Database for cross-device sync
- **No dependencies, no npm, no backend** — works the moment it's uploaded
- **Share links are host-agnostic** — built from `location.origin + location.pathname`, so they work correctly under any GitHub Pages subpath (`/repo-name/`) automatically
- **`.nojekyll`** — tells GitHub Pages to skip Jekyll processing, ensuring the file serves instantly without build delays

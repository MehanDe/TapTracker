# Tap Tracker (cloud sync)

Tracks your Apple Pay taps and syncs them through your own free Firebase project. Sign in on any device and see the same data. It survives clearing Safari.

Files: `index.html` (the app), `firebase-config.js` (your Firebase settings), `firestore.rules` (security rules).

## 1. Create the free Firebase project (about 5 minutes)

1. Go to https://console.firebase.google.com and sign in with a Google account.
2. **Create a project** (any name, e.g. `tap-tracker`). You can turn Google Analytics off.
3. **Build → Authentication → Get started → Email/Password → Enable → Save.**
4. **Build → Firestore Database → Create database.** Choose a location near you (e.g. `northamerica-northeast1` Montréal or `northamerica-northeast2` Toronto), start in **production mode**.
5. In Firestore, open the **Rules** tab, replace everything with the contents of `firestore.rules`, and click **Publish**. This makes your data readable and writable only by you.
6. **Project settings (gear icon) → General → Your apps → Web (`</>`)** → register an app (skip Hosting). Copy the `firebaseConfig` values.
7. Paste those values into `firebase-config.js`.

Stay on the free **Spark** plan. No payment method is needed for this app.

## 2. Deploy on GitHub Pages

1. Create a GitHub repo and upload `index.html`, `firebase-config.js`, `firestore.rules` and this README.
2. **Settings → Pages → Deploy from a branch → `main` → `/ (root)`.**
3. Your app is at `https://<your-username>.github.io/<repo-name>/`.
4. Back in Firebase: **Authentication → Settings → Authorized domains → Add domain** and add `<your-username>.github.io`. Without this, sign-in will fail.

## 3. First sign-in

Open the app in Safari, enter your email and a password, and tap **Create account**. Afterwards, to stop anyone else from creating accounts, go to Firebase **Authentication → Settings → User actions** and turn off **Enable create (sign-up)**. Your own account keeps working, and the security rules already stop anyone from seeing your data.

## 4. Auto-log every tap (iOS Shortcuts)

1. **Shortcuts → Automation → + → Transaction.**
2. Pick the cards to watch, then **Next**.
3. Add **Open URL** with:

   ```
   https://<your-username>.github.io/<repo-name>/?amt=[Amount]&merchant=[Merchant]&card=[Card or Pass]
   ```

   Insert the variables from the Transaction input. Add a **URL Encode** step on Merchant first.
4. Choose **Run Immediately**.

Each tap opens the app in Safari, which saves the tap to your cloud database. If you aren't signed in yet, or are offline, the tap is held on the phone and saved as soon as you sign in or reconnect.

Keep using the app in **Safari** (bookmark it). Safari and a home-screen web app keep separate sign-ins on iPhone.

## Free limits

Firestore free tier: 1 GiB stored, 50,000 reads/day, 20,000 writes/day, 20,000 deletes/day. This app uses a tiny fraction of that.

## Backup

**Export CSV** downloads every transaction. **Import CSV** restores them without creating duplicates.

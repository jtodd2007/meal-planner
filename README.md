# Weekly Meal & Grocery Planner: setup on a PC

You do not need to know any code. You will put these files on GitHub (a free website host), then install the app from the web address you get.

## Part 1: One-time setup (about 10 minutes)

1. **Unzip the files.** Right-click `meal-planner-app.zip` > **Extract All**. You will get a folder with `index.html`, `manifest.json`, `sw.js`, `firebase-config.js`, `README.md`, and an `icons` folder.
2. **Make a free GitHub account** at https://github.com/signup (skip this if you have one).
3. **Create a repository.** Click the **+** at the top right > **New repository**.
   - Repository name: `meal-planner`
   - Choose **Public**.
   - Click **Create repository**.
4. **Upload the files.** On the new repository page click **uploading an existing file**.
   - Open your unzipped folder, select everything inside it (including the `icons` folder), and drag it all onto the GitHub page.
   - Wait for the files to finish loading, then click **Commit changes** at the bottom.
5. **Turn on the website.** Click **Settings** (top of the repository) > **Pages** (left side).
   - Under "Build and deployment" set Source to **Deploy from a branch**.
   - Set Branch to **main** and the folder to **/ (root)**, then click **Save**.
6. **Wait 1 to 2 minutes**, then refresh the Pages screen. A box appears saying "Your site is live at https://YOURNAME.github.io/meal-planner/". That is your app's address. Bookmark it.

## Part 2: Install it like an app

**On your PC (Chrome or Edge):**
1. Open your app address.
2. Click the small **install icon** at the right end of the address bar (a monitor with a down arrow). Or open the browser menu (three dots) > **Install Meal Planner** (Edge: **Apps** > **Install this site as an app**).
3. Click **Install**. It now has its own window, a desktop icon, and a Start menu entry. Right-click it in the taskbar to pin it.

**On your iPhone:** open the address in **Safari** > tap **Share** > **Add to Home Screen** > **Add**.

**On your Android phone:** open the address in **Chrome** > tap the three dots > **Install app** (or **Add to Home screen**).

## Part 3: Every Wednesday

1. When the app asks, rate last week's meals (stars, a note, save favorites).
2. Download the new Meijer ad PDFs.
3. In the app, open **Plan** > **Weekly refresh checklist** > **Copy my Wednesday message for Claude**. Paste it into your chat with Claude and attach the PDFs.
4. Claude sends back a new `index.html`. In your GitHub repository, click **Add file** > **Upload files**, drag the new `index.html` in, and click **Commit changes**. Same name = it replaces the old one.
5. Wait 1 minute, then close and reopen the app. The new meals and prices are there.

## Part 4: Turn on sync (phone and PC stay in step)

This uses Google's free **Firebase** service to hold one shared copy of your data. You do this once, on your PC. It takes about 15 minutes. What syncs: favorites, ratings, pantry, this week's picks, your built grocery list, and the check marks while you shop.

**A. Create the Firebase project**
1. Go to https://console.firebase.google.com and sign in with your Google account.
2. Click **Create a project** (or **Add project**). Name it `meal-planner`. Click **Continue**.
3. Turn **off** Google Analytics (you don't need it), then click **Create project**. Click **Continue** when it finishes.

**B. Turn on Google sign-in**
1. In the left menu click **Build** > **Authentication**, then **Get started**.
2. Click **Google**, switch **Enable** on, pick your email under "Project support email", and click **Save**.
3. Still in Authentication, click the **Settings** tab > **Authorized domains** > **Add domain**. Type `YOURNAME.github.io` (your GitHub name, the same one in your app's address) and click **Add**.

**C. Create the database**
1. In the left menu click **Build** > **Firestore Database** > **Create database**.
2. Pick the location closest to you (for example `nam5 (United States)`), click **Next**, choose **Start in production mode**, and click **Create**.
3. Click the **Rules** tab. Delete everything in the box and paste this exactly:

```
rules_version = '2';
service cloud.firestore {
  match /databases/{database}/documents {
    match /users/{uid} {
      allow read, write: if request.auth != null && request.auth.uid == uid;
    }
  }
}
```
4. Click **Publish**. (These rules mean only you, signed in, can see your data.)

**D. Get your connection settings**
1. Click the **gear icon** next to "Project Overview" > **Project settings**.
2. Scroll to **Your apps** and click the **web** icon `</>`.
3. Nickname: `meal-planner`. Leave "Firebase Hosting" unchecked. Click **Register app**.
4. You will see a block of code containing `const firebaseConfig = { apiKey: "...", authDomain: "...", ... }`. Keep this page open.

**E. Put the settings in your app**
1. In your GitHub repository, click the file **firebase-config.js**, then the **pencil icon** (Edit).
2. Replace each `PASTE...` value with the matching value from Firebase (apiKey, authDomain, projectId, storageBucket, messagingSenderId, appId). Keep the quote marks and commas.
3. Click **Commit changes**. Wait 1 to 2 minutes.

**F. Sign in on each device**
1. Open the app and tap **☁️ Sync** (top of the Plan tab) > **Sign in with Google**. Use the same Google account on every device.
2. The button changes to **☁️ Synced ✓**. Do the same on your phone.

From then on, a meal you pick on your PC shows up on your phone within seconds, and ticking off an item at the store shows on the other device too.

**If sign-in doesn't work**
- "Unauthorized domain": repeat step B3 and make sure the domain matches your address exactly (no `https://`, no `/meal-planner`).
- A pop-up that never appears: allow pop-ups for the site in your browser, then try again.
- iPhone: Google sign-in can be unreliable inside an app added to the Home Screen (Apple restricts it). If it fails there, open the same address in Safari and sign in there, or use the backup code in the Sync window to move your data.

**Backup anytime:** the Sync window also has **Copy backup code**, **Download file**, and **Restore from code**.

## What's in the app

- **Plan:** household size, budget, diet chips, weekly checklist
- **Meals:** 40 new meals and 10 new snacks each week, favorites, per-meal servings, cheaper swaps, Cook mode
- **Deals:** this week's sale items
- **List:** priced grocery list. Check items off, tap "I have it" for pantry items, share or email it, or copy a link so someone else can check items off on their own device.

## Good to know

- Without sync turned on (Part 4), your favorites, ratings, and pantry live only on the device you use. With sync on, they follow you to every device you sign in on.
- The app works offline after the first visit.
- If an update doesn't show up, close the app fully and reopen it, or press Ctrl+F5 in the browser.

# HOW TO GET YOUR APK & EXE — No Flutter Install Needed
## Using GitHub Actions (Free)

---

## What is this?
GitHub is a free website where you upload code, and their computers
automatically build it for you. You get your APK and EXE as a download.
No Flutter, no Android Studio needed on YOUR computer.

---

## STEP 1 — Create a free GitHub account
Go to: https://github.com
Click "Sign up" → use any email → free plan is enough.

---

## STEP 2 — Create a new repository
1. After logging in, click the **+** button (top right) → **New repository**
2. Name it: `rcis-app`
3. Set to **Private** (so your code is not public)
4. Click **Create repository**

---

## STEP 3 — Upload the source code
On the new empty repository page:

**Option A — Upload via website (easiest):**
1. Click **"uploading an existing file"** link
2. Drag and drop ALL files from the `rcis_app` folder
   (including the hidden `.github` folder — you may need to
    show hidden files on your computer first)
3. Click **Commit changes**

**Option B — Use Git (if you know it):**
```bash
cd rcis_app
git init
git add .
git commit -m "Initial commit"
git remote add origin https://github.com/YOURUSERNAME/rcis-app.git
git push -u origin main
```

---

## STEP 4 — Watch it build automatically
1. On GitHub, click the **Actions** tab
2. You will see "Build RCIS App" workflow running
3. Wait 5–10 minutes for Android, 15 min for Windows/iOS
4. Green checkmark ✅ = build successful

---

## STEP 5 — Download your APK and EXE
1. Click on the completed workflow run
2. Scroll down to **Artifacts**
3. Download:
   - **RCIS-Android-APK** → contains your `.apk` files
   - **RCIS-Windows-EXE** → contains your `.zip` with the `.exe`
   - **RCIS-iOS-Build** → unsigned iOS build

---

## Installing the APK on Android
1. Copy the `.apk` file to your phone
2. On your phone: Settings → Security → Allow "Install from unknown sources"
3. Open the APK file → Install
4. Done!

For **arm64-v8a** APK → use this for almost all modern Android phones (2017+)
For **armeabi-v7a** APK → use this for older phones

---

## Firebase (Push Notifications) — Optional
If you want push notifications, before uploading to GitHub:
1. Go to https://console.firebase.google.com
2. Create a project called RCIS
3. Add an Android app with package name: `com.rcis.app`
4. Download `google-services.json`
5. Replace the placeholder `lib/firebase_options.dart` using:
   ```
   dart pub global activate flutterfire_cli
   flutterfire configure
   ```
6. Place `google-services.json` in `android/app/`

If you SKIP Firebase setup, the app still works perfectly —
just without push notifications. The build won't fail.

---

## Need help?
The entire build is free. GitHub gives you 2,000 minutes/month free.
Building this app uses about 15 minutes per build.

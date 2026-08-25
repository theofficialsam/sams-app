# Android Build Setup Guide

Complete step-by-step walkthrough for building S.A.M.S as a native Android APK using Capacitor.

---

## 1. Prerequisites

Install these before starting:

- **Node.js 18+** — https://nodejs.org
- **Android Studio** — https://developer.android.com/studio (installs the Android SDK)
- **Java 17** — bundled with Android Studio; set `JAVA_HOME` if needed
- **Git** — to clone this repo

---

## 2. Clone and install

```bash
git clone https://github.com/YOUR_USERNAME/sams-app.git
cd sams-app
npm install
```

---

## 3. Add the Android platform

```bash
npx cap add android
npx cap sync android
```

This creates the `android/` folder (native Android Studio project). It is git-ignored — regenerate it any time with the commands above.

---

## 4. AndroidManifest.xml permissions

Open `android/app/src/main/AndroidManifest.xml` and add inside `<manifest>`:

```xml
<!-- File access for saving PDFs and backups -->
<uses-permission android:name="android.permission.READ_EXTERNAL_STORAGE" android:maxSdkVersion="32" />
<uses-permission android:name="android.permission.WRITE_EXTERNAL_STORAGE" android:maxSdkVersion="29" />

<!-- Back-button listener via @capacitor/app -->
<!-- (no extra permission needed — registered automatically by the plugin) -->
```

> On Android 13+ (API 33+), storage permissions are no longer required for files saved to app-private directories or shared via the Share Sheet — which is how S.A.M.S saves files. The manifest entries above are for older Android versions only.

---

## 5. App icon

Replace the default Capacitor icon with the S.A.M.S logo:

```bash
# Using the capacitor-assets CLI (recommended)
npm install --save-dev @capacitor/assets
npx capacitor-assets generate --android
```

Place your source icon at `assets/icons/icon-512.png` before running the command. It will generate all the required `mipmap-*` density variants automatically.

Alternatively, manually copy the icons from `assets/icons/` into `android/app/src/main/res/mipmap-*/`.

---

## 6. App name and ID

Edit `android/app/src/main/res/values/strings.xml`:

```xml
<string name="app_name">S.A.M.S</string>
```

The application ID (`com.sams.attendance`) is set in `capacitor.config.json` and applied to `android/app/build.gradle` automatically during `cap sync`.

---

## 7. Build a debug APK

```bash
npx cap open android
```

In Android Studio:
- Connect a device or start an emulator
- Click **▶ Run** (Shift+F10) to install a debug build

---

## 8. Build a release APK

In Android Studio:

1. **Build → Generate Signed Bundle / APK**
2. Choose **APK**
3. Create or select a keystore (keep this file safe — you need it for every update)
4. Select **release** build variant
5. Click **Finish**

The signed APK appears at `android/app/release/app-release.apk`.

---

## 9. Updating the app

After changing `index.html`:

```bash
npx cap sync android
# Then rebuild in Android Studio or use:
npx cap run android
```

---

## 10. Troubleshooting

| Problem | Fix |
|---------|-----|
| `npx cap add android` fails | Make sure `ANDROID_HOME` or `ANDROID_SDK_ROOT` env var points to your SDK |
| White screen on device | Enable WebView debugging: set `webContentsDebuggingEnabled: true` in `capacitor.config.json`, inspect via `chrome://inspect` |
| File save doesn't work | Confirm `@capacitor/filesystem` and `@capacitor/share` are installed and `cap sync` was run after |
| Back button closes app instantly | Confirm `@capacitor/app` is installed and `cap sync` was run |
| Icons not showing | Run `npx capacitor-assets generate` or copy icons manually |

# S.A.M.S — Scholastic Attendance Management System

> A fully offline-first, single-file attendance app built for Android (via Capacitor) and any modern browser or PWA. No server, no account, no internet required after first install.

---

## Features

- **Class & Subject management** — create, rename, and delete classes and subjects with full cascading data updates
- **Student roster** — add, edit, delete students; bulk import via Excel (.xlsx/.xls)
- **Daily attendance** — mark Present/Absent per student per date, with topic notes and undo
- **Bulk toggle** — mark all students Present or Absent in one tap (with correct visual feedback)
- **Reports** — month-by-month or all-time overall attendance percentage per student
- **PDF export** — export any report as a PDF (shares via native Share Sheet on Android)
- **JSON backup** — full backup or single-class backup; restore from backup file
- **Copy attendance** — duplicate one subject's records into another with per-date selection and undo
- **Edit/Delete classes & subjects** — rename or delete with full data cascade (students, attendance, attendance IDs)
- **Custom dropdown system** — fully themed, keyboard-accessible select replacement matching the dark UI
- **Offline-first** — Tailwind CSS, Font Awesome icons, and the SAMS logo are all inlined; no CDN calls needed at runtime (xlsx/html2canvas/jsPDF still load from CDN on first use but degrade gracefully if unavailable)
- **Android back button** — proper navigation: closes modals → jumps home tab → double-press to exit
- **Read cache** — `getAllData()` caches IndexedDB reads in memory and auto-invalidates on any write, reducing redundant reads on low-spec devices

---

## Repository structure

```
sams-app/
├── index.html              ← The entire application (HTML + CSS + JS, self-contained)
├── manifest.json           ← PWA web app manifest
├── capacitor.config.json   ← Capacitor native build config
├── package.json            ← npm dependencies for Capacitor
├── assets/
│   └── icons/              ← PWA / launcher icons (generated from logo)
│       ├── icon-48.png
│       ├── icon-96.png
│       ├── icon-192.png
│       └── icon-512.png
└── docs/
    └── SETUP.md            ← Android build setup guide
```

---

## Running in the browser (instant, no install)

Just open `index.html` directly in any modern browser. All data is stored in the browser's IndexedDB — nothing leaves the device.

```bash
# Optional: serve locally for PWA features (service worker, installability)
npm install
npm run serve
# → opens at http://localhost:3000
```

---

## Building the Android APK (Capacitor)

### Prerequisites

- Node.js 18+
- Android Studio (with Android SDK, API 33+)
- Java 17+

### Steps

```bash
# 1. Install dependencies
npm install

# 2. Add the Android platform (first time only)
npx cap add android

# 3. Sync web assets into the native project
npx cap sync android

# 4. Open in Android Studio to build / run
npx cap open android
```

In Android Studio: **Build → Generate Signed Bundle / APK** to produce a release APK.

> See [`docs/SETUP.md`](docs/SETUP.md) for a detailed walkthrough including AndroidManifest permissions and signing.

---

## Required native plugins

These are listed in `package.json` and installed via `npm install`. After adding them, always run `npx cap sync`.

| Plugin | Purpose |
|--------|---------|
| `@capacitor/app` | Android back button handling |
| `@capacitor/filesystem` | Save PDF/JSON files to device storage |
| `@capacitor/share` | Open the native Share Sheet after saving a file |

---

## Data storage

All data is stored in **IndexedDB** under the app's origin, in three object stores:

| Store | Key | Contents |
|-------|-----|----------|
| `students` | `studentKey` (`className_rollNumber`) | Student roster |
| `attendance` | `id` (`date_class_subject_roll`) | Per-session attendance logs |
| `metadata` | `key` | Structural map (class → subjects list) |

Because IndexedDB is sandboxed per-app/origin on Android, data persists across app restarts and is never accessible to other apps. Use **Backup → Export JSON** regularly to keep an external copy.

---

## Tech stack

| Layer | Technology |
|-------|-----------|
| UI Framework | Tailwind CSS v3 (compiled to static CSS, inlined) |
| Icons | Font Awesome 6 Free (subset inlined as base64 woff2) |
| Storage | IndexedDB (via raw IDB API, no wrapper library) |
| Native bridge | Capacitor 6 |
| PDF export | html2canvas + jsPDF (CDN, graceful offline degradation) |
| Excel import | SheetJS/xlsx (CDN, graceful offline degradation) |

---

## License

MIT — free to use, modify, and distribute.

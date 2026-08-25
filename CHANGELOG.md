# Changelog

All notable changes to S.A.M.S are documented here.

---

## [1.0.0] — Initial Release

### Core features
- Class and subject management with Academic Structure view
- Student roster with add / edit / delete and bulk Excel import
- Daily attendance sheet with Present/Absent radio marking and topic notes
- Bulk mark-all Present or Absent with correct visual feedback
- Undo last attendance save
- Monthly report and Overall (all-months) attendance percentage per student
- Individual student summary report
- PDF export for any report view (native Share Sheet on Android, download in browser)
- JSON backup export — full backup or single selected class
- JSON backup import with backward-compatible studentKey backfill
- Excel attendance import with date-serial detection

### Android / PWA
- Capacitor 6 integration with @capacitor/filesystem + @capacitor/share for file saves
- Android back button handler: closes modals → home tab → double-press to exit
- Offline-first: Tailwind CSS, Font Awesome 6 icons, and SAMS logo all inlined (no CDN required at runtime)

### UI
- Custom SamsSelect dropdown system replacing all native selects
- SAMS logo on splash screen and header
- Dark-theme-only design throughout
- Custom animated splash / loading screen
- Toast notification system
- Two-step confirmation modal for destructive operations (wipe)

### Data integrity
- Composite studentKey (className_rollNumber) prevents cross-class roll number collisions
- Cascading rename and delete for classes and subjects (updates student records, attendance IDs)
- All async writes properly awaited before UI refresh (eliminates race-condition stale reads)
- Race-condition double-submit protection on Save Student and Save Attendance buttons
- Case-insensitive duplicate detection for class and subject names

### Performance
- IndexedDB read cache with automatic invalidation on writes
- `db.transaction` monkey-patched post-init to intercept all readwrite calls for cache invalidation
- `content-visibility: auto` on off-screen tab sections
- Redundant DB reads in `evaluateContextLock` cascade eliminated by cache

### Copy attendance
- Copy one subject's attendance records into another subject in the same class
- Per-date chip filter — select only the dates you want, others are ignored
- Undo last copy — full snapshot + restore

### Edit classes and subjects
- Rename a class — cascades to all students (className, studentKey) and all attendance (className, id)
- Delete a class — cascades to delete all students and attendance for that class
- Rename a subject — cascades to all attendance (subjectName, id)
- Delete a subject — cascades to delete all attendance for that subject (students unaffected)

# Attend2Go — Competition Technical Prototype

## What's included
- `index.html`: responsive teacher attendance and school-office demo
- `manifest.webmanifest`: installable web-app metadata
- `sw.js`: service worker for static asset caching/offline app shell

## Run locally
For a quick UI preview, open `index.html` in a modern browser. Attendance data is stored in browser local storage.

For actual PWA/offline-cache testing, serve the folder over HTTPS or localhost:
1. Put the folder on a static host with HTTPS (or use localhost).
2. Open `index.html` in Chrome on Android.
3. Use the browser menu → Install app / Add to Home screen.
4. Load once while online, then switch to airplane mode and reopen.

## Demo walkthrough
1. Teacher view opens with 40 fictional students.
2. Tap P or A beside any student; changes save locally.
3. Search by name or roll number.
4. Use Mark all present, then review exceptions.
5. Open Office dashboard to see today's absentees and export CSV.
6. The sample starts with two fictional students who were absent on the previous two school days. Today's sample attendance makes their 3-day alert visible.
7. Tap “Show demo alert scenario” to trigger another example.
8. Tap Sync now while online to demonstrate clearing the pending-sync queue.

## Important prototype limitations
- Sync is simulated; no backend server or real multi-device sync is connected.
- Demo student names are fictional. Do not enter real student data into this prototype.
- The demo treats Monday–Friday as school days and does not yet support school holidays.
- Authentication, role-based access, encryption at rest, audit trails, consent/retention controls, and server-side conflict resolution must be implemented before any real-school pilot.
- For a real deployment, use an authenticated API, per-school tenant isolation, encrypted transport, minimal student data, access logs, backups, and explicit retention/deletion policies.
- Three-day alerts should be reviewed by a teacher before contacting a family; absence alone should not trigger punitive decisions.

## Suggested production architecture
Android/PWA client → encrypted local database + sync queue → HTTPS API → PostgreSQL
                                                        ↘ alert worker → teacher notification
                                                        ↘ office dashboard

## Competition demo success metrics to measure
- Median time to mark 40 students
- Percentage of attendance records saved offline
- Successful sync rate after reconnection
- Time from sync to office dashboard visibility
- Accuracy of consecutive-school-day alerts
- Teacher usability feedback

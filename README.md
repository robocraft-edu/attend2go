# Attend2Go

**Offline-first school attendance for low-connectivity schools.**

Attend2Go is a competition-stage technical prototype for schools with 400–1,200 students, Android phones for teachers, a small Windows computer lab, and unreliable 2G–4G connectivity.

## Current prototype

- Mobile-friendly teacher attendance screen with 40 fictional students
- Present / absent controls, search, and mark-all-present action
- Local browser storage so the demo continues without a network connection
- Office dashboard with daily counts and absentee list
- CSV export for spreadsheet workflows
- Demonstration of a three-consecutive-school-day follow-up alert
- Installable PWA shell and static asset caching

## Run locally

Open `index.html` in a modern browser for a quick preview. For service-worker/PWA testing, serve this folder over HTTPS or localhost, open it in Chrome on Android, and choose **Install app** / **Add to Home screen**.

## Demo walkthrough

1. Open the teacher view.
2. Mark students present or absent; changes are saved in this browser.
3. Search by fictional name or roll number.
4. Open **Office dashboard** to review today's absentees.
5. Export the absentee list as CSV.
6. Review the three-day absence follow-up examples.
7. Try **Sync now** to see the simulated sync queue clear.

## Important: prototype, not production software

This version has **no real server, account system, multi-device synchronisation, or production-grade encryption**. The Sync button is a simulation. Browser local storage is used for the demo. Do not enter real student or family data.

Before any real-school pilot, implement authentication, role-based access, school-level data isolation, encrypted transport and storage, server-backed sync, conflict handling, audit logs, backups, retention/deletion controls, and an approved parent-notification process.

## Design principles

- Attendance entry must never wait for internet access.
- A missing or unsubmitted register must be **UNKNOWN**, not automatically ABSENT.
- Consecutive absence alerts count school days, excluding weekends and configured holidays.
- Alerts are prompts for a teacher to review, not automatic punitive decisions.
- Parents should receive concise Hindi-first communication; WhatsApp sharing or print can be reviewed by staff. SMS should be optional and quota-controlled.
- A family contact may be linked to multiple students using an internal random reference, but one child's message must not reveal sibling information.

## Repository structure

- `index.html` — teacher and office demo
- `manifest.webmanifest` — PWA metadata
- `sw.js` — static app-shell service worker
- `docs/ARCHITECTURE.md` — proposed production architecture
- `docs/PRIVACY_AND_TEST_DATA.md` — privacy and synthetic-data rules

## License

No license has been added yet. All student names and records in the demo are fictional.

# Attend2Go — Proposed Production Architecture

## Deployment context
Target schools may have 400–1,200 students, Android phones for teachers, a small Windows computer lab and intermittent 2G–4G internet. Parents may share phones and often prefer Hindi. The current prototype is browser-only; this document describes a future production design.

## Components
1. Teacher PWA / Android client — mobile-first attendance entry.
2. Local encrypted database — cached roster and dated attendance drafts.
3. Durable sync queue — retries uploads after connectivity returns; idempotency keys prevent duplicates.
4. School API — authenticates users and validates school, class, teacher and date permissions.
5. PostgreSQL — school-scoped classes, students, calendar and attendance.
6. Office dashboard — browser UI for Windows PCs; UTF-8-with-BOM CSV export for Excel.
7. Alert worker — checks consecutive school-day absences and creates teacher-review tasks.
8. Parent communication workflow — Hindi-first templates, staff-reviewed WhatsApp sharing, print fallback and optional quota-limited SMS.

## Workflow
- Teacher marks attendance without waiting for internet.
- Each change is stored locally and queued for sync.
- When online, client sends small batches with unique idempotency keys.
- Server validates school scope, class assignment, date and roster version.
- Office dashboard updates after accepted sync.
- Alert worker evaluates configured school days and holidays.
- Teacher reviews any follow-up task and decides whether to contact family.

## Attendance states
Use PRESENT, ABSENT and UNKNOWN (not submitted / unavailable). Never convert missing data into absence. Corrections must retain an audit event with actor, timestamp and reason.

## Suggested database tables
- schools(id, name, timezone, settings_json)
- users(id, school_id, role, display_name, auth_subject)
- classes(id, school_id, name, academic_year)
- teacher_class_assignments(user_id, class_id)
- students(id, school_id, class_id, roll_code, display_name, active)
- family_contacts(id, school_id, family_ref, preferred_language)
- student_family_links(student_id, family_contact_id, relationship_label)
- school_calendar(id, school_id, date, is_school_day, label)
- attendance(id, school_id, student_id, class_id, school_date, status, updated_by, updated_at, version)
- sync_batches(id, school_id, client_id, idempotency_key, received_at, result)
- audit_events(id, school_id, actor_id, entity_type, entity_id, action, created_at, reason)
- followup_tasks(id, school_id, student_id, trigger_date, status, assigned_to, resolved_at)

Use random internal identifiers. Do not store Aadhaar, student photos, biometrics, precise location or unrelated family information.

## Sync and conflict policy
- Every write requires authenticated identity and school scope.
- Enforce uniqueness for student + class + school date.
- Keep server and client timestamps separately.
- Never silently overwrite conflicts; let authorised staff resolve them and log the resolution.
- Retry with backoff; retain unsent items until server acknowledgement.
- Show sync status and last successful sync time.

## Three-day alert logic
Count configured school days, excluding weekends and declared holidays. UNKNOWN or missing attendance must not count as absence. Create a teacher-review task, not an automatic parent message or disciplinary action.

## Network and launch gates
Cache the app shell and current roster. Compress small sync batches. Avoid heavy libraries and unnecessary network requests. Before pilot: unit-test calendar logic, test duplicate/delayed/reordered sync, test role permissions and tenant isolation, test backups and restore, and test shared-device and low-bandwidth conditions. Start with synthetic data; use approved minimum real data only after school and privacy/security review.
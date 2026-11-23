# Attend2Go — Deployment and Privacy Notes

## Target operating environment

- School size: approximately 400–1,200 students.
- Teachers use Android phones; no teacher laptops are assumed.
- Office staff use Windows PCs in a shared computer lab.
- Connectivity is intermittent 2G–4G.
- Existing records include Excel attendance and handwritten fee books. This prototype does not implement fee management.
- Parents may prefer Hindi and one phone may be shared across siblings.
- SMS should be optional and tightly budgeted; provide teacher-reviewed WhatsApp sharing and printable notices.

## Proposed architecture

1. Mobile-first PWA teacher client.
2. Local attendance store and an offline sync queue.
3. Authenticated HTTPS sync API.
4. PostgreSQL database with school/tenant isolation.
5. Windows-friendly office dashboard with CSV import/export for Excel workflows.
6. Alert worker evaluating consecutive scheduled school days.
7. Teacher-reviewed Hindi parent communication by manual WhatsApp share or print; optional quota-controlled SMS disabled by default.

The current repository contains only a front-end demonstration. The server, database, authentication, real sync, alert worker, and parent messaging are not implemented.

## Attendance rules

- Attendance marking must not wait for internet access.
- Missing or unsubmitted attendance is UNKNOWN / NOT SUBMITTED, never automatically ABSENT.
- Three-day alerts count scheduled school days, excluding weekends and configured holidays.
- Missing submissions must not be interpreted as absences.
- Alerts prompt a teacher to verify the situation; they are not automatic disciplinary decisions.
- Concurrent edits require an explicit conflict policy and audit trail.

## Family contact model

A family contact may be associated with multiple students. Use an opaque random internal family_ref. A message about one child must not reveal another child's attendance. Collect only minimum necessary contact information, with clear notice and an appropriate school-approved basis for use.

## Data minimisation and security

- Use synthetic student records in development, demos, screenshots, and videos.
- Never put real student data, phone numbers, parent details, or school records in this public repository.
- Do not collect Aadhaar numbers, student photographs, biometrics, precise location, or unrelated family information.
- Require authentication and role-based access for teachers, office staff, and administrators.
- Enforce school-level tenant isolation on every server request.
- Encrypt traffic in transit and protect stored data with appropriate encryption and key management.
- Keep access/change audit logs; restrict exports.
- Define backups, retention periods, correction procedures, and deletion processes.
- Obtain school approval and provide appropriate notices before any real deployment.
- Review applicable Indian child-data and privacy requirements with the school and qualified advisers before a pilot.

## Low-cost parent communication

- Default to a teacher-reviewed Hindi message.
- Prefer manual WhatsApp sharing or printed notices where appropriate.
- If SMS is enabled, apply a school-defined monthly quota and prevent uncontrolled sending.
- Never send automated messages solely from an unverified absence alert.

## Validation plan

Test only with synthetic data and representative low-connectivity conditions:

| Measure | Test |
|---|---|
| Speed | Median time to mark 40 learners |
| Offline reliability | Records retained after closing/reopening without internet |
| Sync | Queue behaviour and duplicate prevention after reconnecting |
| Office visibility | Time between successful sync and dashboard update |
| Alert correctness | Streaks across weekends, holidays, missing submissions, and corrections |
| Usability | Teacher task completion on low-cost Android devices |
| Export | CSV opens correctly in Excel, including Hindi text |

Use UTF-8 CSV with a byte-order mark (BOM) if needed for the chosen Excel workflow.

## Release gate before a real-school pilot

Do not use real student data until the production system has server-side authentication and authorization, tenant isolation, encrypted local storage, real sync with retries/idempotency/conflict resolution, configured school calendars, audited alert logic, access logs, backup/restore tests, retention/deletion controls, school approval, privacy review, and teacher training.

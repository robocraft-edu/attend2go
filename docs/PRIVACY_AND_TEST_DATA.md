# Privacy, Safety and Test Data Rules

## Public repository
This repository is public. Only fictional sample records and synthetic scenarios belong here. Never commit real student names, roll numbers, phone numbers, parent details, attendance histories, screenshots containing real records, school registers, credentials, API keys or access tokens.

## Data minimisation
A production deployment should collect only fields needed for attendance and approved follow-up. Do not collect Aadhaar numbers, student photographs, biometrics, precise location, personal device identifiers or unrelated family details.

## Family contact handling
- Use a random internal family reference instead of exposing a parent's phone number in ordinary attendance views.
- One family contact may link to multiple students, but a message about one child must not disclose sibling information.
- Keep contact details out of public exports and teacher views unless explicitly required and authorised.

## Access and security
Before real deployment, require individual authenticated accounts, role-based permissions, strict school-level data isolation, encrypted network traffic and storage, short shared-device sessions, audit logs, tested backups, retention/deletion procedures and school approval with appropriate family notices. Review applicable child-data and privacy obligations with the school before launch.

## Parent notifications
Use concise Hindi-first, staff-reviewed templates. WhatsApp sharing should be initiated and checked by authorised staff; print remains a fallback. SMS must be optional, quota-controlled and disabled by default. Never send an automatic punitive message solely because an absence streak is detected.

## Attendance correctness
UNKNOWN means attendance has not been submitted or is unavailable. It is not ABSENT. Three-day alerts count only confirmed absences on configured school days; weekends and declared holidays are excluded. Alerts are prompts for a teacher to verify records and follow up appropriately.

## Fictional data
All names in the demo are fabricated placeholders. Never substitute real student information, even temporarily, in screenshots, issue reports, pull requests or demonstrations.
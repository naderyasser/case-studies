# Biometric Attendance — desktop edition — [repo](https://github.com/naderyasser/meena-time)

**Kind:** a one-time-purchase Windows app with the same calculation rules as the cloud attendance system

## What it does
- Reads **ZKTeco fingerprint devices** over TCP/UDP (port 4370)
- Works fully offline on one local SQLite file, saved atomically after every change, with backups
- Shifts, groups, departments, holidays, leaves and permissions; closes payroll periods
- Exports Excel and PDF reports

`Electron` `sql.js (SQLite)` `node-zklib` `xlsx` `Chromium PDF`

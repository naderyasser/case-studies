# White-label Learning Platforms — [el3aref.com](https://el3aref.com)

**Kind:** multi-tenant SaaS that gives teachers and tutoring centres in Egypt their own learning platform
**Role:** everything — architecture, backend, UI, AI, infrastructure, operations

## The idea
A teacher signs up, picks a plan and a design template, and minutes later has a complete platform on
`<name>.el3aref.com` or their own domain: protected video courses, timed exams, printed access codes,
online and transfer payments, centre attendance, parent reports and an AI study assistant.

## Numbers
| | |
|---|---|
| Code | ≈ 93k lines of Python + ≈ 5.5k lines of TypeScript |
| Automated tests | 2,200+ (unit, integration, security scenarios) |
| Design templates | 22 complete looks (colours, type, geometry, ornaments) |

## How it is built
**Multi-tenancy.** One PostgreSQL schema per customer (django-tenants). A platform is resolved from its
subdomain, a custom domain proved by a TXT record, or an app header. Two-step self-signup with Google,
provisioning in the background (Celery, idempotent, retried), real plan limits (students, courses,
storage), trials with a grace day, suspension, archive with a 30-day grace, and a data export that opens
correctly in Arabic Excel.

**Roles.** Shared accounts, roles per platform (owner, admin, teacher, assistant with scoped abilities,
student, parent). JWT for the app with refresh rotation and blacklist, sessions for the web.

**Video.** Private Cloudflare R2 storage, short-lived signed URLs, automatic encrypted HLS with a key
that rotates every minute ([django-vidlock](https://github.com/naderyasser/django-vidlock), open source),
per-lesson and per-code view limits, device limits and download-tool detection.

**AI study assistant.** Streaming answers that know the student (grade, courses, weak topics) and the
open lesson. Hybrid RAG over the syllabus — chunks split on the author's headings, **bge-m3** embeddings
chosen by measurement (rank-1 recall 5/12 → 11/12 against the obvious alternative), 0.75 semantic +
0.25 lexical scoring, Arabic prefix handling — plus self-hosted web search. Retrieval is scoped to what
the student paid for, every answer is logged with its passages and scores, and a 300-question sweep
runs after any syllabus change.

**Money.** Printed codes with batches, wallet, the owner's own Paymob account, bank/wallet transfers
with receipt review, course sales, coupons. Webhooks verify HMAC, lock the row and tolerate retries.

**Operations.** Docker Compose (web, worker, beat, PgBouncer, PostgreSQL, Redis), GitHub Actions
(lint + 3 test shards + deploy), **blue/green deploys with no downtime**, error tracking tagged by
platform, nightly backups to R2, wildcard + per-domain certificates, and an operator console in
Next.js with team roles, MRR, support tickets and an audit log.

**Security.** A full audit pinned by tests: privilege escalation, cross-platform account takeover,
CSRF/CORS between subdomains, stored XSS, receipt access, webhook forgery.

`Django` `DRF` `django-tenants` `PostgreSQL` `Redis` `Celery` `Next.js` `Cloudflare R2` `Docker` `GitHub Actions`

# Multi-branch Gym ERP + members' app

**Kind:** a full ERP for a gym chain — staff dashboard, API and a members' PWA
**Role:** everything, across three repositories

## The idea
One system runs the whole operation: members and subscriptions, check-ins, branches, staff and
trainers, finance, sales targets, bookings, online and recurring payments, WhatsApp/SMS/push — plus a
gamification engine and an automated CRM that catches members about to cancel.

## Numbers
| | |
|---|---|
| Backend | ≈ 36.5k lines of Python · 16 Django apps · 1,200+ tests |
| Staff dashboard | ≈ 16k lines of React/TypeScript |
| Members' app | ≈ 5.5k lines of React/TypeScript (PWA) |

## Architecture
- **Two APIs with separate identities:** `/api/v1/` for staff, `/client/v1/` for members — a member's
  token is refused on every staff route and the other way round.
- **Branch-scoped from the ground up:** every row belongs to a branch, chosen with `X-Branch-Id`.
- Roles and permissions, teams, payroll and staff attendance; signed payment webhooks; OpenAPI docs.

## Modules
Core (audit log, soft delete, branch scoping, dashboards) · accounts · members · memberships ·
finance (PDF reports) · goals · **competition** (points, levels, streaks, challenges, badges, rewards,
teams) · **crm** (churn-risk detection and win-back campaigns) · personal-training marketplace with
trainer commissions · payments (Paymob and Fawry, recurring, tokens only) · booking with waitlists ·
messaging (templates and automated flows).

## Nightly automation (Celery Beat, Cairo time)
Expire subscriptions → notifications → attendance streaks → levels → team challenges →
**churn-risk detection** → win-back messages → daily branch KPIs → sales targets → badges →
**collect due recurring payments**.

`Django` `DRF` `PostgreSQL` `Celery` `React` `TypeScript` `Tailwind` `PWA`

# Arora Classes — Website Architecture

Architecture and API design for the website of **Arora Classes**, a Class 9–12 coaching centre in **Kitchlu Nagar, Ludhiana, Punjab**.

## Live document

**https://YOUR-USERNAME.github.io/arora-classes-architecture/**

## The problem

10–15 enquiry calls a day, most during class hours, most unanswered. The ones that connect ask the same four things: timings, fees, seat availability, faculty. Publishing that removes most of the calls; a form captures the rest.

## What's inside

[`index.html`](index.html) — five diagrams, each with its tables:

1. **System Overview** — three tiers, trust boundaries, every connector labelled
2. **Frontend Layer** — ten routes, rendering mode, the API each one calls
3. **Backend Layer** — five-stage pipeline, four services, endpoints, contract, status codes
4. **Database Layer** — two tables, one bucket, full DDL, RLS
5. **Deployment Layer** — where each part runs and where secrets live

Plus key decisions and a five-phase roadmap.

[`Arora-Classes-Architecture.pdf`](Arora-Classes-Architecture.pdf) — print-ready system diagram.

## Tech stack

| Layer | Choice |
|-------|--------|
| Framework | Next.js 14 (App Router) + TypeScript |
| UI | Tailwind CSS |
| Validation | Zod |
| Database + storage | Supabase (PostgreSQL) |
| Email | Resend |
| Hosting | Vercel, ap-south-1 |

**Running cost: ₹0/month** — everything inside free tiers at this scale.

## API surface

| Method | Route | Auth |
|--------|-------|------|
| POST | `/api/enquiry` | public |
| GET | `/api/updates` | public |
| POST · PATCH · DELETE | `/api/updates` | admin |
| POST | `/api/upload` | admin |
| GET · PATCH | `/api/enquiries` | admin |
| POST | `/api/auth/login` · `/logout` | public · admin |

## Data model

- `updates` — title, body, image_url, file_url, is_published, timestamps
- `enquiries` — name, phone, student_class, message, status, created_at

Files go to a `update-files` bucket; the database stores only the URL. Batches, fees and faculty stay in `content/*.ts` — the owner doesn't edit them, which removes three tables and three admin screens.

## Three decisions

- **Enquiries are stored *and* emailed.** Email is the least reliable step, so the row is the source of truth and a delivery failure never fails the request.
- **RLS enabled with no policies.** Blocks the anonymous key entirely; all access goes through the server with the service key.
- **Everything in Mumbai.** Co-locating functions and database removes a cross-continent round trip.

## Roadmap

- [ ] 1 — Static site with fixed content
- [ ] 2 — Enquiry form: validation, persistence, email
- [ ] 3 — Login and text-only updates
- [ ] 4 — Image and PDF attachments
- [ ] 5 — Admin enquiry inbox with status

Each phase is independently deployable.

## Status

Architecture complete. Implementation not started.

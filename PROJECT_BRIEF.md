# CivicFix / CivicProof — Project Brief

## One-liner
"CivicProof is not another complaint portal. It makes civic-service closure verifiable, public, and measurable."

## What it is
A civic-tech app where citizens report local infrastructure issues (potholes, broken lights, garbage, water leaks) via photo. The core differentiator isn't just reporting — it's **verified resolution**. A "closed" ticket has to prove it's actually fixed.

## The Core Loop (differentiator)
1. Citizen snaps photo → GPS auto-tagged → issue submitted
2. AI clusters duplicate reports around the same location instead of creating noise
3. Every status change goes into a tamper-evident, append-only timeline
4. To close an issue: admin uploads before/after photo + location match + community confirms the fix
5. Dashboard tracks repeat failures, overdue issues, neighborhood resolution rates

---

## Target Users
| User | Need |
|------|------|
| Citizen / Reporter | Quickly report an issue near them without friction |
| Community member | Confirm/upvote existing reports to signal urgency |
| Authority / Admin | See a prioritized, filterable queue of issues and update status |

---

## MVP Feature Priority

### Must-Have
- Auth — email/OTP or Google sign-in; separate admin login
- Submit Issue — photo + category + GPS + optional description
- Duplicate Detection & Clustering — coordinate-based matching
- Public Map & List View — pins on map, filter by category/status/distance
- Tamper-Evident Status Timeline — append-only: Reported → Acknowledged → In Progress → Resolved
- Verified Resolution Flow — before/after photo + location match + community verification
- Admin Dashboard — filterable queue, repeat-failure highlights, neighborhood resolution rates

### Should-Have
- Community confirmation / upvotes
- AI classification & severity detection from photo

### Nice-to-Have
- Push/email notifications on status change
- Heatmap of issue density / repeat-failure hotspots
- Public impact stats page

---

## User Flows

### Citizen
Sign in → Report Issue → Photo → Category → Confirm GPS → Submit → Track on "My Reports"

### Community
Browse map → Open issue → "Confirm — I see this too" → optional comment/photo

### Admin
Login → View queue → Open report → Update status (logged to timeline) → Verified Resolution Flow to close

### Verified Resolution Flow
1. Admin uploads after-photo from original location
2. System checks location match
3. Issue → "Pending Verification", before/after shown publicly
4. Community verifies or disputes
5. Threshold met → status locked as "Resolved"
6. Disputed fixes stay visible, feed repeat-failure tracking

---

## Data Model

### User
`id, name, email, role (citizen | admin)`

### Issue
`id, reporter_id, category, severity, photo_url, description, latitude, longitude, cluster_id, status, confirmation_count, resolution_before_photo_url, resolution_after_photo_url, resolution_verified_count, created_at, updated_at`

**Status enum:** `reported → acknowledged → in_progress → pending_verification → resolved`

### Confirmation
`id, issue_id, user_id, created_at`

### StatusUpdate (append-only, never edited/deleted)
`id, issue_id, status, note, photo_url, updated_by, created_at`

**Tamper-evidence:** each entry hashed as `hash(previous_hash + entry_data)` — no update/delete endpoints.

---

## Tech Stack
- **Frontend:** React (web-first) + Leaflet (maps)
- **Backend:** Node/Express or FastAPI
- **Auth + DB + Storage:** Firebase or Supabase (one platform)
- **AI classification:** optional, not a blocker

## Build Priority
`submit → map → status timeline → admin update → verified resolution flow` — end-to-end first, AI is a bonus.

---

## Commit Plan (~25–35 commits)

| Slice | Commits |
|-------|---------|
| Project setup (scaffold, deps, env, DB schema) | 3–4 |
| Auth (Google/OTP, role-based routing) | 2–3 |
| Issue submission (photo, GPS, category, submit, clustering) | 4–5 |
| Public map & list view (pins, filters, detail page) | 3–4 |
| Tamper-evident timeline (append-only table + hash chain + UI) | 2–3 |
| Community confirmation (endpoint + UI) | 2 |
| Verified resolution flow (before/after, location match, verify/dispute, lock) | 4–5 |
| Admin dashboard (queue, filters, stats) | 3–4 |
| Polish / demo prep (UI cleanup, seed data, README) | 2–3 |

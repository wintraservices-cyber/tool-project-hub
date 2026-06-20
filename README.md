# ArrowheadX Project Portal

**GitHub Repo:** [wintraservices-cyber/ProjectDiary-andreas](https://github.com/wintraservices-cyber/ProjectDiary-andreas)

Managed by [ArrowheadX AI](https://arrowheadx.ai)

---

## Overview

A single-file, multi-project management portal. No frameworks, no build step, no dependencies — just one HTML file that runs in any browser.

Supports **multiple clients/projects**, **PIN-based sign-in** with per-manager project visibility, and syncs through **JSONBin.io** + **localStorage**.

---

## File Structure

```
/
├── index.html           → Redirects to the portal on load
├── vercel.json          → Vercel routing config
├── project-portal.html  → The entire app
└── README.md            → This file
```

---

## Sign In

- Type your **name** and **PIN** (no dropdown — just type and go)
- Default admin account: **Darwin** / PIN `1234` — change this immediately after first deploy via **⚙ Settings → 🔑 Change My PIN**
- Sessions last for the current browser tab/session (`sessionStorage`) — closing the browser requires signing in again

### Roles
- **Admin** — sees and edits every project, manages team access
- **Manager** — only sees projects explicitly assigned to them, but has **full edit access** to anything they can see

---

## The Tabs

### 🗇 All Projects
Landing page. Grid of project cards filtered to what the signed-in manager can see. Click a card to open it, or **+ New Project** to create one (auto-assigned to whoever creates it, if not admin).

### 🗂 Project Hub
Client info, description, goals, work items, milestones, team contacts, open questions.

### 📓 Daily Log
Entry form: Category · Task/Description · Status (Planning / In Progress / Completed / Issues) · Date · Notes.
**💾 Save Daily Log** pushes all entries into that project's Progress Report.

### 📊 Progress Report
Full task tracker, auto-grouped by **calendar week (Monday-start)** computed from each task's date.
Filter by status, search, add/edit/delete tasks.
**⬇ Export / ⬆ Import** here are scoped to just this project's tasks — useful for backing up or sharing a single project's report without touching anything else. Import supports **merge** (skip duplicates) or **replace**.

### 🔐 Team Access (admin only)
Opened via **⚙ Settings → 🔐 Team Access** — not a main tab, since it's account-level, not project-level.
Add managers, edit any manager's PIN inline, assign/reassign which projects they can see, remove managers.

---

## ⚙ Settings Menu

Everything else lives in one dropdown, organized by section:

| Section | Items |
|---|---|
| **Account** | 🔑 Change My PIN · 🔐 Team Access *(admin only)* |
| **Cloud Sync** | ☁ Refresh from Cloud · ⚙ Sync Settings |
| **Data** | ⬇ Export All Data · ⬆ Import All Data · ⎙ Print Current Tab · ↺ Reset All Data |

`⏻ Logout` stays as its own topbar button since it's a primary action.

---

## Data & Sync

Everything lives in **one object, one localStorage key** (`arrowheadx_portal_v1`), synced through **one JSONBin record**.

```json
{
  "portal": {
    "projects": { "slug": { "hub": {...}, "dl": {...}, "pr": {...} } },
    "activeProject": "slug",
    "managers": { "id": { "name": "...", "pin": "...", "role": "...", "assignedProjects": [...] } }
  }
}
```

### First-time cloud setup
1. Go to [jsonbin.io](https://jsonbin.io) → create a free account
2. Avatar → **API Keys** → copy **Master Key**
3. Open the portal → **⚙ Settings → ⚙ Sync Settings** → paste Master Key → **Connect**
4. A Bin is created automatically — save the **Bin ID** shown in the alert
5. On any other device: **⚙ Settings → ⚙ Sync Settings** → enter same Master Key + Bin ID

### Data flow
```
Edit anything
      ↓
Saved to localStorage instantly (offline-safe)
      ↓
Pushed to JSONBin after 2 seconds
      ↓
Any device → ⚙ Settings → ☁ Refresh from Cloud
```

---

## Deployment

Hosted on **Vercel**, auto-deploys from this GitHub repo.

### How to update
**GitHub website:** open `project-portal.html` → ✏ edit → commit → Vercel deploys in ~30s
**GitHub Desktop:** clone repo → edit locally → commit → push → Vercel deploys automatically

### URL shortcuts
| Path | Destination |
|---|---|
| `/` | Redirects to portal |
| `/portal` | project-portal.html |

---

## Tech Stack

| Layer | Technology |
|---|---|
| Frontend | Vanilla HTML + CSS + JavaScript |
| Fonts | Google Fonts — Cormorant Garamond + Inter |
| Storage | localStorage + JSONBin.io REST API |
| Hosting | Vercel (static) |
| Auth | PIN-based, client-side only (not cryptographically secure — practical access control, not a real auth system) |

---

**ArrowheadX AI** · [arrowheadx.ai](https://arrowheadx.ai)

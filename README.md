# Settl — Automated Group Payment Collection

> **Pay once. Chase no one.**

Settl is a mobile app prototype designed for the UAE market that eliminates the friction of collecting shared payments from groups. Built for sports organisers, social group admins, and anyone who regularly splits costs — Settl automates the entire collection process through open banking, so organisers never have to chase, remind, or follow up again.

---

## Table of Contents

- [Overview](#overview)
- [Problem Statement](#problem-statement)
- [Key Features](#key-features)
- [App Flow](#app-flow)
- [Screens & Functionality](#screens--functionality)
- [UAE PASS Integration](#uae-pass-integration)
- [Open Banking Consent Model](#open-banking-consent-model)
- [Prototype Notes](#prototype-notes)
- [Tech Stack](#tech-stack)
- [How to Run](#how-to-run)
- [Demo Guidance](#demo-guidance)
- [Project Status](#project-status)

---

## Overview

Settl is a **click-through prototype** built as a single HTML file, simulating the full end-to-end experience of a group payment collection app. The prototype is designed to look and feel like a native iOS app, rendered inside a phone frame in the browser.

The app targets the UAE market and integrates with **UAE PASS** (the UAE's national digital identity platform) for authentication, and demonstrates how **open banking APIs** can be used to pull payments automatically from participants once they give consent.

---

## Problem Statement

When organising recurring group activities — padel sessions, football games, team dinners — one person always ends up chasing everyone else for money. This involves:

- Sending payment reminders manually
- Awkward follow-ups when people forget
- Tracking who has and hasn't paid
- Re-doing all of this every single week

Settl solves this entirely. Once participants consent once, Settl collects automatically after every session — no reminders, no awkward messages, no manual reconciliation.

---

## Key Features

### For Organisers
- **Create events** with name, cost, date, and venue
- **Add participants** by searching the Settl directory (by name or phone number) or sending invite links to non-users
- **Automatic cost splitting** calculated in real time as players are added
- **Automated collection** via open banking — payments pulled after each session
- **Recurring sessions** — set weekly, fortnightly, or monthly cadence so the organiser never needs to touch the app again
- **Live status dashboard** — see who has approved, who has paid, and how much has been collected
- **Session history** — track all past sessions and amounts collected
- **Organiser dashboard** — total games organised, total collected, outstanding amounts

### For Participants
- **One-time consent** — approve auto-debit once and never think about it again
- **Transparent terms** — fixed amount per session shown clearly before consent
- **Full control** — revoke consent at any time
- **No app required for invite** — non-users receive a Settl invite link

---

## App Flow

The complete end-to-end flow works as follows:

```
Login
  └── UAE PASS authentication (2-stage verification flow)
      └── Dashboard (Your Groups)
            ├── View existing events → Event Status Screen
            │     ├── See approval/payment status per participant
            │     ├── Preview what members see (consent screen)
            │     ├── Complete session (demo)
            │     └── Cancel event / recurring series
            │
            └── Create new event
                  ├── 1. Event Details (name, cost, date, venue)
                  ├── 2. Add Players (search directory or invite)
                  ├── 3. Enable Automation (toggle auto-collect)
                  ├── 4. Set Recurring (weekly / fortnightly / monthly)
                  └── 5. Status Screen → awaiting member approvals
                              └── Members approve → Collection Active
                                        └── Complete Session → Summary
```

---

## Screens & Functionality

### Login Screen
The entry point of the app. Users can:
- **Sign in with UAE PASS** — triggers a two-stage verification flow mimicking the real UAE PASS app experience (loading bar → spinner overlay → dashboard)
- **Sign in with username/password** — standard credential login with validation (min 3 chars username, min 6 chars password)
- **Create an account** — placeholder for registration flow

### Dashboard (Your Groups)
The home screen after login, showing:
- **Quick stats bar** — Total Collected, Pending, Active events at a glance
- **This Week** section — upcoming events within the next 7 days
- **Last Weekend** section — recently completed events with stats
- Each group card shows: event name, venue, date, participant count, collected vs total, progress bar, and collection method (auto or manual)
- **Create Event** button to start a new event

### Create Event
A form collecting:
- Event name
- Total cost (AED)
- Date (today or future only — validated)
- Venue
- All fields are required with inline error messages

### Add Players
- **Split banner** — shows total cost and per-person share, updating live as players are added or removed
- **Find on Settl** — search the Settl user directory by name or phone number; matching users can be added instantly
- **External invite** — if no Settl account is found, the organiser can send an invite link; the person joins Settl and consents to payment
- **Contacts list** — pre-populated list of regular players that can be toggled on/off
- Selected players are highlighted with a teal border

### Automation Screen
- Toggle to enable or disable automated collection
- When enabled: payments are pulled automatically after each session — no reminders needed
- When disabled: organiser manually marks payments as received
- Benefits listed: no reminders, no follow-ups, automatic reconciliation, recurring support

### Recurring Setup
- Choose frequency: **Weekly**, **Fortnightly**, or **Monthly**
- Preview of next session date shown dynamically
- Skip option available for one-time events

### Event Status Screen
Shown after creating an event or tapping an existing one. Two states:

**Awaiting Approvals (auto-collect)**
- Shows each participant with an "Awaiting" or "Approved" chip
- Organiser can tap to simulate a participant approving (for demo purposes)
- Links to "Member View" to preview the consent screen as a participant

**Collection Active (all approved)**
- Shows all participants as approved
- Collection is live — Settl handles everything automatically
- "Complete Session" button simulates the collection cycle completing

**Manual Collection**
- Organiser marks each payment manually using "Mark paid" / ✓ Paid buttons
- Progress bar and collected/pending amounts update in real time

### Member View (Consent Screen)
What participants see when they receive an invite:
- Event name and organiser details
- Amount to be collected per session
- Explicit open banking consent terms (5 items including legal acknowledgement)
- "I consent — Approve auto-debit" button
- "Decline" option

### Session Complete Screen
Shown after a session is completed:
- Success animation
- Event summary: name, total collected, participant count, manual reminders sent (always 0)
- Next session date (if recurring)
- "Back to Dashboard" button

### Activity Tab
Full history of all events:
- Status chips: Complete, Awaiting, In Progress, Cancelled
- Session count for recurring events
- Collection method indicator
- Tap any event to open a detailed receipt

### Receipt Screen
Detailed view of a past or active event:
- Gradient header with event name, date, venue, and collection method
- Cost summary: total, per person, participant count
- Per-participant payment/approval status
- Session history for recurring events (S1, S2, S3... with dates and amounts)

### Profile / Organiser Dashboard
- **Organiser Dashboard banner** — Total Games, Total Collected, Outstanding amounts
- Profile card with role
- Stats: events organised, total collected, average group size
- Settl Pro subscription status

### Groups Tab
- In Progress events
- Completed events
- Empty states for both sections

---

## UAE PASS Integration

UAE PASS is the UAE's national digital identity platform. Settl's login flow simulates the real integration:

1. User taps "Sign in with UAE PASS"
2. **Stage 1** — A UAE PASS-branded verification screen appears with the fingerprint logo, Arabic branding ("الهوية الرقمية"), and a segmented dot loading indicator that fills over ~6 seconds
3. **Stage 2** — The login screen reappears with a grayed-out overlay and a spinning loader for ~4 seconds, simulating the app receiving the verified identity
4. User lands on the dashboard, authenticated

This flow mirrors the actual UAE PASS experience seen in apps like RTA and TAMM.

---

## Open Banking Consent Model

Settl's core legal and security mechanism is the **explicit consent screen** shown to each participant before auto-collection begins. This is not just a UX element — it represents the legal basis for pulling payments via open banking APIs.

When a participant approves, they confirm:

1. They authorise Settl to pull the fixed amount from their account after each session via open banking
2. This acceptance is their **legal consent** for automated payment collection
3. Payments are collected automatically — no further action required
4. They can revoke consent at any time by contacting the organiser
5. The amount is fixed per session with no surprises

This consent model is the critical security and legal function of the prototype — it demonstrates how open banking auto-debit can be implemented responsibly and transparently in a consumer-facing UAE app.

---

## Prototype Notes

This is a **front-end only prototype** — no backend, database, or real API calls. All data is:
- Seeded on load with 4 example events (Friday Padel, Sunday Football, Thursday Badminton, Saturday Padel)
- Stored in JavaScript memory during the session
- Reset on page reload

**Simulated behaviours:**
- UAE PASS login uses timed animations to mimic the real flow
- "Tap to approve" buttons simulate participants approving on their own devices
- "Complete Session" simulates Settl collecting payments automatically
- Directory search queries a local array of mock Settl users

**Seeded data includes:**
- 1 recurring weekly event (Friday Padel) with 3/4 approvals — shows "Awaiting approvals" state
- 1 manual collection event (Sunday Football) with 1/3 paid — shows "In Progress" state
- 1 auto-collect event pending all approvals (Thursday Badminton)
- 1 completed event from last weekend (Saturday Padel) — appears in "Last Weekend" section

---

## Tech Stack

| Layer | Technology |
|---|---|
| Language | HTML5, CSS3, Vanilla JavaScript |
| Rendering | Single-file SPA (no frameworks) |
| Styling | Inline CSS + CSS classes, CSS custom properties |
| Animations | CSS keyframes (`slideIn`, `popIn`, `pulse`, `spinLoop`) |
| Icons | Inline SVG |
| Fonts | System font stack (`-apple-system`, `SF Pro Display`, `Segoe UI`) |
| No dependencies | Zero external libraries or CDN imports |

The prototype is intentionally dependency-free for maximum portability — it runs by opening the HTML file directly in any modern browser.

---

## How to Run

```bash
# Clone the repository
git clone https://github.com/your-username/settl.git

# Open in browser — no server required
open index.html
```

Or simply drag `index.html` into any browser window.

**Tested on:** Chrome, Safari, Firefox, Edge (latest versions)

---

## Navigating the Demo

For the best demo experience, follow this path:

1. **Login** using UAE PASS (watch the full verification flow) or enter any username (3+ chars) and password (6+ chars)
2. **Dashboard** — tap **Friday Padel** to see a recurring event with 3/4 approvals
3. Tap **"Tap to approve"** next to Omar to see the full approval → collection active state
4. Tap **"Preview what members see →"** to see the consent screen
5. Tap **"I consent — Approve auto-debit"** and return to the status screen
6. Tap **"Complete Session (demo)"** to simulate collection and see the summary screen
7. Return to dashboard and tap **"+ Create Event"** to walk through the full creation flow
8. Check the **Activity**, **Groups**, and **Profile** tabs

---

## Demo Guidance — What's Simulated & Why

Because this is a single-device prototype, some interactions that would normally happen
on a *participant's* phone have been brought onto the organiser's screen for demo purposes.
Here's what to know before exploring:

---

### "Tap to approve" buttons
**What it looks like:** On the Event Status screen for auto-collect events, each participant
row shows a coral "Tap to approve" button next to their name.

**What it actually means:** In the real app, this button would not exist for the organiser.
It is purely a demo mechanism to simulate a participant opening the app on *their own device*
and tapping "I consent — Approve auto-debit" on their consent screen.

**Why it's here:** Since we cannot show two phones simultaneously, this button lets the
reviewer simulate what happens on the participant's end — triggering the approval state
change and watching the organiser dashboard update in real time as approvals come in.

---

### "Complete Session (demo)" button
**What it looks like:** A teal button at the bottom of the Event Status screen once all
participants have approved.

**What it actually means:** In the real app, Settl would trigger this automatically after
the session date passes — the organiser never taps anything. The payments would be pulled
in the background via the open banking API without any organiser action required.

**Why it's here:** To demonstrate what the post-collection summary screen looks like and
to show the full cycle completing. Tapping it simulates Settl doing its job automatically.

---

### "Mark paid" / "✓ Paid" buttons (manual collection events)
**What it looks like:** On events where auto-collect is turned off (e.g. Sunday Football),
each participant row has a "Mark paid" button the organiser can toggle.

**What it actually means:** This is genuine organiser functionality — in manual mode,
the organiser really would mark payments as received themselves. This is the fallback
for users who do not want auto-debit enabled.

**Why it matters:** It demonstrates that Settl supports both automated and manual
collection, giving organisers the choice.

---

### "Preview what members see →" link
**What it looks like:** A small teal link at the bottom of the participant list on the
status screen.

**What it actually means:** This opens the Member View — the exact screen a participant
would see on their device when they receive a payment request from this organiser.

**Why it's here:** To give the reviewer full visibility into both sides of the experience
without needing a second device. In production, this screen would only ever appear on
the participant's phone.

---

### The participant consent screen ("I consent — Approve auto-debit")
**What it looks like:** After tapping the preview link, you see a screen titled
"Member View" with consent terms and two buttons — approve or decline.

**What it actually means:** Tapping "I consent — Approve auto-debit" here simulates a
*participant* approving on their end. It will mark one pending participant as approved
and return you to the organiser status screen, where you'll see the approval count
increase.

**Why this matters:** This is the critical legal and security function of the app —
the moment a participant consents is the moment Settl has legal basis to auto-collect
from them via open banking.

---

### Recurring session counter
**What it looks like:** Friday Padel shows "4 sessions completed" in the Activity tab
receipt, with a full session history (S1–S4) listed with dates.

**What it actually means:** This data is pre-seeded to demonstrate what a long-running
recurring event looks like over time. Each time "Complete Session" is tapped, the
session count increments, the date rolls forward by one week, and approvals reset —
showing the recurring cycle in action.

---

### The directory search
**What it looks like:** On the Add Players screen, typing a name or phone number into
the search field returns matching Settl users.

**What it actually means:** In production this would query a live user database. Here it
searches a local array of 7 mock Settl users (Khalid, Nour, Ravi, Fatima, Tom, Mariam,
José). If no match is found, a "Send Settl invite link" option appears — simulating the
onboarding flow for people not yet on the platform.

---

### Login validation
**What it looks like:** The login form shows error messages if fields are blank, too short,
or missing.

**What it actually means:** Any username (3+ characters) and any password (6+ characters)
will successfully log in — there is no real authentication. The validation is there to
demonstrate the UX of a production login form, not to gate access.

---

### Data persistence
**Important:** All data lives in browser memory only. Refreshing the page resets
everything back to the original seeded state (the 4 example events). There is no
database or backend — this is intentional for a prototype of this scope.

---

## Project Status

This prototype was built as an academic submission demonstrating:

- A complete end-to-end user flow for a fintech product
- UAE market-specific integrations (UAE PASS, AED currency, open banking consent)
- Mobile-first design within a realistic iOS phone frame
- Automated payment collection UX with explicit legal consent mechanisms

**Version:** 1.0 — Submission prototype  
**Market:** UAE  
**Category:** Fintech / Group Payments / Open Banking

---

*Settl — No reminders. No follow-ups. Just settled.*

# RefractID

> **Zero-surveillance campus access. Auditable emergency mustering.**
>
> Team PRISMATIC · TEF 26 Case Study Competition · Trust Track · October 2026

RefractID is a privacy-first campus safety architecture presented not as a technology stack but as a **governance story**: a campus that refuses the false choice between surveillance and blindness, told through a 90-second live demo of a student checking into a lab, a fire alarm triggering, identity decrypting only for the affected zone, a muster resolving in real time, and an audit receipt landing on the student's phone.

This repository contains the polished single-file prototype that stages that story. It runs offline, in any modern browser, with no backend, no database, and no installation.

---

## Table of Contents

- [The Problem](#the-problem)
- [The Governance Story](#the-governance-story)
- [What This Prototype Is](#what-this-prototype-is)
- [Quick Start (60 seconds)](#quick-start-60-seconds)
- [The Auto Demo](#the-auto-demo)
- [What You Will See On Screen](#what-you-will-see-on-screen)
- [Project Structure](#project-structure)
- [Technical Notes](#technical-notes)
- [What This Prototype Deliberately Does Not Have](#what-this-prototype-deliberately-does-not-have)
- [Honest Limitations](#honest-limitations)
- [For Judges](#for-judges)
- [Design System](#design-system)
- [Attribution](#attribution)
- [Team](#team)
- [License](#license)

---

## The Problem

Campus safety systems today force a bad trade-off.

**Surveillance:** RFID muster gates log every door swipe with a name, matric number, and timestamp into a central database. Expensive hardware. A permanent record of where every student walked. A surveillance state in the name of safety.

**Blindness:** Paper logbooks and manual guard checks are cheap, but they are forgeable, impossible to query in a real emergency, and useless in an actual evacuation. If a fire breaks out in a late-night lab, security has no idea whether three students or thirty are trapped inside.

The Trust Track asks for a third option: *how can student identity help create a safer campus while respecting privacy?*

RefractID answers by **separating daily verification from emergency identification**.

---

## The Governance Story

RefractID is built around three acts. Every visual element in the prototype belongs to one of them.

### Act I — The Promise (Normal Operations)

A student checks into a lab. The system verifies only that an enrolled, lab-certified student is present. On the security dashboard, an anonymous occupancy counter ticks up. Security sees "3 verified occupants," not three names. No movement trails. No identity data collected. The privacy ledger on the student's phone stays at zero.

### Act II — The Break (Emergency)

A fire alarm sounds. The system does **not** decrypt automatically. It requires two independent signatures — Campus Security **and** the Emergency Commander — and it decrypts only the active tokens for the affected zone. Names, matric numbers, and medical flags appear on the muster manifest. The floor plan turns red. The occupants become specific, addressable people who need to be evacuated.

### Act III — The Receipt (Accountability)

As students reach the muster point and mark themselves safe, their rows flip from red to green in real time. The unaccounted counter decrements. Every transition is written to an immutable audit trail. And on the student's phone, a timestamped audit receipt appears: who accessed their identity, for which zone, under which alarm, and when. The promise is repaired in public.

> RefractID is not a mockup of a technology. It is a **live rehearsal of a governance promise being made, broken under dual authorization, and repaired in public** — in 90 seconds, in one file, with zero clicks.

---

## What This Prototype Is

- A **single HTML file** (`index.html`) with all CSS and JavaScript inline.
- **Two synced views** in two browser windows: a mobile-shaped **Student Wallet** on the left, a desktop **Security Command Dashboard** on the right.
- **Zero backend.** The two views communicate via the browser's `BroadcastChannel` API.
- **Zero external dependencies.** The Google Fonts stylesheet is optional; the prototype falls back to system fonts if offline.
- A **governance-process simulator** — not a cryptographic proof-of-concept.

It exists for one reason: to make the privacy paradox visible, and to make the resolution of that paradox auditable, in the time it takes to pitch a slide.

---

## Quick Start (60 seconds)

1. Clone the repository: `git clone https://github.com/Evan-Lim/refractid-prototype.git`
2. Enter the folder: `cd refractid-prototype`
3. Serve the file locally: `python3 -m http.server 8000`
4. Open `http://localhost:8000` in **two separate browser windows** (not just two tabs).
5. In one window, click **Auto Demo** in the top bar.

That is the entire demo.

### Why a local server and not just "open the file"?

The two windows communicate using `BroadcastChannel`, which is only guaranteed to work on the same **HTTP origin**. Opening `index.html` directly from disk works in some browsers and not in others. A local HTTP server makes the demo reliable on every machine. GitHub Pages works equally well — see [Deployment](#deployment) below.

### Deployment

The prototype is hosted as a static site. To redeploy:

1. Push `index.html` to `main` on GitHub.
2. Go to **Settings → Pages**, set **Source** to `Deploy from a branch`, branch `main`, folder `/ (root)`.
3. The site is live at `https://Evan-Lim.github.io/refractid-prototype/`.

No build step. No dependencies. The deployed site is the file itself.

---

## The Auto Demo

The single most important interaction in the prototype is the **Auto Demo** button in the top bar. It removes human error from the most scrutinized 90 seconds of the pitch by driving the entire flow on a fixed timeline, with zero clicks needed beyond the initial button press.

**Timeline:**

| Time | What Happens On Screen |
|------|-------------------------|
| 0:00 – 0:02 | Window resets to Normal mode. Occupancy reads 0. Mode pill is violet. |
| 0:02 – 0:08 | First student checks in. Occupancy ticks to 1. A violet dot appears in Lab 1. |
| 0:08 – 0:14 | Second and third students check in. Occupancy reaches 3. |
| 0:14 – 0:22 | Alarm triggers. Background shifts to red. Muster manifest appears. Aisyah Rahman shows as UNACCOUNTED. |
| 0:22 – 0:30 | Rows 2 and 3 appear. Floor plan dots turn red. Unaccounted counter reads 3. |
| 0:30 – 0:42 | First student marked safe. Row flips to green. Audit receipt appears on the student's phone. |
| 0:42 – 0:52 | Second student marked safe. Audit trail grows. |
| 0:52 – 1:02 | Muster completes. Unaccounted reads 0. Six audit entries visible. |
| 1:02 – 1:10 | Hold on the split screen. |

Clicking Auto Demo in **either** window drives **both** windows, because every state change is broadcast over the channel. Narrate over it; do not click through the individual buttons during the pitch.

---

## What You Will See On Screen

### Global Chrome

- **Mode pill** — reads `Normal Operations` in violet, `Break-Glass Active` in red. Tells the judge which act they are in.
- **Auto Demo** — runs the 90-second flow.
- **Reset** — returns both windows to Act I.

### Student Wallet (left column, mobile-shaped)

- **Verified Status** — enrollment and lab-certification badges, with the caption *"Your identity is masked."*
- **Anonymous Access Token** — a rotating hex token simulating a zk-SNARK proof.
- **Check in to Blok L Lab** — the single primary action.
- **Privacy Ledger** — two rows that stay at zero during normal operations, plus a yellow audit-receipt card that appears only after Break-Glass.
- **I am safe at DK7** — appears only after the alarm, so the student can mark themselves safe.

### Security Dashboard (right column)

- **Occupancy KPI** — a large violet gradient number for verified occupants, plus a red unaccounted count (an em-dash during normal operations).
- **SVG floor plan of Blok L** — three labs, a corridor, an exit to DK7. Each checked-in student appears as a dot that changes color based on state: violet for anonymous, red for unaccounted, green for confirmed-safe.
- **Active Tokens (Encrypted)** — the locked-token list. Each entry reads `Token 8f9a-c2e1...` with an `ENCRYPTED` label. Clicking a token does nothing. That is the point.
- **Emergency Break-Glass** — the alarm trigger, followed by the live muster manifest once pressed.
- **Immutable Audit Trail** — a scrolling column of timestamped events, each with a truncated SHA-style hash. It grows in real time as the story progresses.

---

## Project Structure

**Root of the repository:**

- `index.html` — the entire prototype, single file, all CSS and JS inline
- `README.md` — this file
- `LICENSE` — MIT

That is the whole repository. There is no `package.json`, no `node_modules`, no build pipeline, no server code.

---

## Technical Notes

### Two-window communication

The student wallet and the security dashboard communicate using the browser's `BroadcastChannel` API on the channel name `refractid-demo`. Any state mutation — check-in, alarm, mark-safe, reset — is serialized and posted to the channel. Both windows receive the update and re-render. This gives the appearance of a live shared session without any server, database, or network traffic.

### State model

The entire prototype state is a single object:

- `occupants` — an array of `{ id, name, matric, medical, room }` records, where `id` is a random 16-character hex string acting as the anonymous token.
- `breakGlass` — a boolean indicating whether the alarm is active.
- `safe` — a `Set` of occupant IDs who have confirmed safe at DK7.
- `auditLog` — an array of `{ time, action, hash }` records, prepended on every state change.

Every action mutates this object, calls `render()`, and calls `sync()` to broadcast the new state.

### SVG floor plan

The floor plan is drawn in a `viewBox="0 0 400 200"` SVG. Occupant dots are appended as `<circle>` elements into a `<g id="occupantDots">` group on every render. Dot positions are computed from the occupant's assigned room index and the occupant's position within that room.

### Auto Demo

The Auto Demo is implemented as a sequence of `setTimeout` calls stored in an array so they can be cancelled if the user presses Reset mid-demo. It calls the same `checkIn()`, `breakGlass()`, and `markSafe()` functions the manual buttons call — it is not a separate code path, just a scripted sequence of the same actions.

---

## What This Prototype Deliberately Does Not Have

Every feature that would have made the prototype **technically more impressive** but **narratively weaker** has been excluded on purpose:

- **No backend.** A server adds three failure points (port, CORS, database) with zero narrative gain in a 90-second demo.
- **No database.** Nothing in the pitch depends on persistence across sessions.
- **No ML pipeline.** The judging panel evaluates governance design, not anomaly detection.
- **No 3D scene.** The 90-second flow does not need a 3D camera; a clean SVG floor plan conveys the story faster.
- **No user accounts.** The demo has three fixed occupants; the point is the flow, not the login screen.
- **No real cryptographic circuit.** The ZKP is simulated. The prototype's job is to make the governance decisions visible, not to prove the math.

Each of these exclusions saves 4–40 hours of build time and removes a deployment failure point, without moving a single judging criterion.

---

## Honest Limitations

This is a **governance simulator**, not a production cryptographic system.

- The zk-SNARK tokens shown in the prototype are simulated. A production build would generate real proofs on the student's device using an established library such as Circom / SnarkJS, as demonstrated by projects like ZKTap.
- The Break-Glass dual-signature is simulated by a single button. A production build would require cryptographic approval from two independent systems, with a formal key-management ceremony and a policy for rotating authorization roles.
- The immutable audit trail is displayed as a scrolling list of hashes. A production build would write to an append-only ledger or a signed log with a public verification endpoint that students could independently check.
- Offline fallback (peer-scan of a physical card, matric pin entry at the muster point) is described in the proposal but not implemented in the prototype.

These are engineering and governance tasks, not research unknowns. Stating them openly is more credible than pretending the prototype is a finished system.

---

## For Judges

The judging criteria are mapped in the [full proposal document](docs/refractid-proposal.pdf), but here is the summary view:

| Criterion (20%) | Where to Look |
|---|---|
| **Problem Understanding & Track Fit** | The three acts above. Each layer maps directly to access, faster verification, emergency coordination, accountability, and privacy. |
| **Impact & User Value** | 40+ minutes of manual roll-call reduced to under 60 seconds of digital manifest. Anonymous occupancy preserved during normal operations. |
| **Feasibility & Affordability** | Zero hardware capex per door. Runs on existing UM Wi-Fi and student smartphones. Under RM 5,000/year in server costs, versus RM 500,000+ for RFID muster gates. |
| **Responsible & Sustainable Design** | PDPA 2010-aligned by architecture: data minimization in Act I, purpose limitation in Act II, automatic audit notification in Act III. Medical flags unlock only for paramedics. Offline peer-scan fallback planned. |
| **Innovation & Pitch Clarity** | The combination of zero-knowledge daily anonymity with auditable Break-Glass mustering under multi-signature authorization has no direct commercial equivalent. The live demo makes the governance flow visible in 90 seconds. |

The core thesis, if you remember only one sentence:

> **We are not claiming to have invented zero-knowledge proofs or emergency mustering. We are claiming to have turned them into a governance story a student would consent to — a promise, a bounded break under dual authorization, and a receipt that lands on the student's phone before they reach the muster point.**

---

## Design System

| Element | Value | Purpose |
|---|---|---|
| Background | Radial gradient from `#0f172a` to `#06090e` | Deep obsidian, feels like infrastructure software |
| Accent (violet) | `#a78bfa` | Normal operations, verified, anonymous |
| Accent (red) | `#ef4444` | Alarm state, unaccounted |
| Accent (green) | `#10b981` | Safe, confirmed at muster point |
| Accent (amber) | `#f59e0b` | Audit receipts, immutable-log entries |
| Typography | Plus Jakarta Sans (with system fallbacks) | Clean, modern, professional |
| Corner radius | 8–14 px | Soft, not playful |
| State transitions | 0.18s – 0.5s ease | Visible but not distracting |
| Glass panels | `backdrop-filter: blur(18px)` | Layered depth without 3D |

The color system is a **state machine**: violet during normal operations, red during Break-Glass, green for confirmed-safe students, amber for audit records. A judge who has seen only the CSS would still know which act they are in.

---

## Attribution

RefractID stands on the shoulders of several adjacent efforts. None of them is a direct competitor; each proves that part of the RefractID design is technically demonstrable:

- **ZKTap (ETHGlobal)** — demonstrates that zk-SNARKs can be compressed into an NFC tap to prove a student claim without revealing personal data. Proves the selective-disclosure interaction is technically feasible.
- **Alert Enterprise Guardian Mustering** — demonstrates that emergency roll-call software is a mature category, with real-time dashboards, missing-person identification, and offline mobile roll-call devices. Proves the mustering workflow is a real operational need.
- **Break-Glass** — a standard IAM concept for temporary emergency access, commonly implemented in enterprise and healthcare systems. RefractID's contribution is not the mechanism but its binding to zk-SNARK tokens, zone-level scope, and student-facing audit receipts.
- **PDPA 2010 (Malaysia Personal Data Protection Act)** — the data-minimization and purpose-limitation language that informs the Responsible Design argument.
- **Plus Jakarta Sans** — the typeface used throughout the prototype, by Tokotype, licensed under the SIL Open Font License.

None of these projects is affiliated with Team PRISMATIC. RefractID's contribution is architectural integration and governance design in a high-privacy-sensitivity campus context.

---

## Team

**Team PRISMATIC** — TEF 26 Case Study Competition, Trust Track.

We are Year 1 Engineering students at Universiti Malaya. The competition brief asked how a verified student identity could make a campus safer while respecting privacy. RefractID is our answer: not a tracking system with a privacy setting, but a governance architecture that keeps its promise to the student visibly, verifiably, and in public.

For questions, contact the team through the TEF 26 organizing committee's WhatsApp group.

---

## License

The prototype code is released under the MIT License. You may use, modify, and redistribute it freely, including in commercial projects, provided the copyright notice is retained.

The ideas, narrative, and governance design described in this README and in the accompanying proposal document are the original work of Team PRISMATIC and are submitted as part of the TEF 26 Case Study Competition. If you reuse them, please attribute.

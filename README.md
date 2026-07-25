# Sean's Royal Kitchen

*Automated weekly meal planning system · Established June 2026*

---

## What This Is

A fully automated pipeline that runs every Friday and builds a personalized 5-dinner menu, writes individual recipe files, creates a shopping list, schedules dinner calendar events with Google Drive recipe links, refreshes a live dashboard, and version-controls everything to GitHub — all informed by Sean's taste ratings and an evolving taste profile.

---

## Current Week

**Week of 2026-07-27** (Mon Jul 27 – Sun Aug 2)

*Authoritative week and dish lineup come from `System/Current_Week.md` (`ACTIVE_WEEK`), including its per-dish status annotations. The on-disk menu file is `Menu_Week_of_2026-07-27.md`; the table below reflects the week as it stands in the ledger — no annotations, all five dishes stand as planned, none carried and none dropped.*

| Dish | Style | Night | Protein | Calories | Status |
|------|-------|-------|---------|----------|--------|
| Jamaican Jerk Chicken Thighs with Coconut Rice & Black Beans | Caribbean | Mon 07/27 | ~45g | ~590 | Scheduled 6:30 PM · new (Caribbean is new to the rotation) |
| Philly Cheesesteak Stuffed Peppers | American Comfort | Tue 07/28 | ~44g | ~470 | Scheduled 6:30 PM · new |
| Bang Bang Salmon Rice Bowls | Asian-Fusion | Wed 07/29 | ~42g | ~580 | Scheduled 6:30 PM · new (quickest dish — 25 min) |
| Vietnamese Lemongrass Pork Meatball Bowls | Vietnamese | Thu 07/30 | ~40g | ~540 | Scheduled 6:30 PM · new (Vietnamese is new to the rotation) |
| Spanish Shrimp & Chorizo Paella | Spanish | Sat 08/01 | ~43g | ~590 | Scheduled 7:00 PM · new (weekend build — 45 min, Spanish is new to the rotation) |

A clean slate: five brand-new dishes, zero recycles and zero early-reuse. Average ~42.8g protein · ~554 calories per serving — every dish clears 40g and stays under 600. Five distinct proteins (chicken thighs, ground beef, salmon, ground pork, shrimp + chorizo) and five distinct cuisines, three of them new.

**Critic rules honored:** the new **yogurt cap** (~1 yogurt-forward sauce per week) — this week has **zero**, with five different sauce bases instead (jerk paste, Worcestershire pan glaze, chili-mayo, nuoc cham, saffron sofrito). No whole pork chops, no stir-fry-with-fries, no tzatziki.

**Calendar:** the week is wide open — the only commitment is Mon 07/27's midday WGU mentor call (12:10–12:25 PM), which doesn't touch dinner. Fri 07/31 and Sun 08/02 left free.

**Previously cooked (week of 2026-07-20)** — the slate the Surveyor asks Sean to rate on Sunday 07/26 and the Critic processes Friday 07/31. **Six dishes**, all cooked, no drops: Creamy Cajun Shrimp Pasta *(carried from 07-13, finally cooked Mon 07/20)*, Ginger-Sesame Turkey Lettuce Wraps, Italian Sausage White Bean & Spinach Skillet, Weeknight Butter Chicken, Lemon-Garlic Butter Scallops with Asparagus & Orzo, and Chimichurri Flank Steak with Charred Corn & Tomato Salad.

**On the bench:** Cajun Honey-Butter Shrimp Bowls (dropped 06-29, never cooked) remains the only early-reuse candidate — passed over this week because Cajun shrimp pasta was on the table Monday.

---

## The Team

| Employee | Task ID | Schedule | Role |
|----------|---------|----------|------|
| 👑 The Kitchen Manager | `the-manager` | Daily ~9 PM | Checks and balances on all employees. Reconciles the ledger daily against the Google Calendar 🍽️ events and Drive `Menu_Adjustment_*` docs. Approves mid-tier changes. Escalates failures to Sean. Sends Wednesday rating nudge if the week is still unrated. |
| 🔧 The Developer | `kitchen-developer` | 1st Friday 6 AM | Monthly system review. Auto-fixes minor issues. Proposes change requests. Co-owns skill drafting via `System/Proposed_Skills/`. |
| 🎭 The Critic | `meal-critic-weekly` | Friday 8 AM | Reads the previous week's ratings, updates the taste profile, writes Lessons Learned for the Chef. |
| 🗄️ The Archivist | `kitchen-archivist` | Friday 4:30 PM | Archives the finished week's files to `Archive/` before the Chef overwrites them. |
| 🍳 The Chef | `weekly-kings-menu` | Friday 5 PM | Builds the coming week's 5 dishes, recipe files, and shopping list; rolls the `Current_Week.md` pointer (preserving status annotations); refreshes the dashboard. |
| 📅 The Scheduler | `kitchen-scheduler` | Friday 7:30 PM | Assigns the active dishes to free evenings; creates calendar events with Google Drive recipe links. |
| 📜 The Scribe | `kitchen-scribe` | Friday 7:45 PM | Refreshes this README and drops the commit-message trigger for the Windows host GitHub sync. |
| 📬 The Surveyor | `meal-surveyor` | Sunday 7 PM | Owns `Rate_This_Week.md` (seeds it with the previous week's cooked, unrated dishes); refreshes the standalone rating artifact; sends Monday 9 AM email + ntfy nudge. |

---

## Single Source of Truth — `System/Current_Week.md`

Every task reads `Current_Week.md` to decide which week it operates on — **never** by guessing from "the most recent `Menu_Week_of_*.md`". It holds:

- **`ACTIVE_WEEK` / `ACTIVE_DISHES`** — the Monday and dishes currently being cooked and scheduled.
- **`PREVIOUS_WEEK` / `PREVIOUS_DISHES`** — the week most recently finished, which the Surveyor asks Sean to rate.
- **Dish status annotations** — Sean adjusts plans mid-week, and the ledger records it per dish: `(CARRIED FROM YYYY-MM-DD)` (moved in from an earlier week; counts as served when actually cooked), `(DROPPED YYYY-MM-DD — not cooked)` (removed and never cooked; not surveyed, exempt from the no-repeat window), and `(RATED)` (already rated; the Surveyor skips it). Every task honors these, and the Kitchen Manager reconciles them daily against the calendar and Drive.

Each Friday the Chef builds a fresh slate of 5 dishes and rolls the ledger forward: the old ACTIVE becomes PREVIOUS (annotations intact), and the newly built week becomes ACTIVE. No dish from the last 3 weeks repeats, except a Critic-recommended 4+ week-old favorite — and never-cooked (dropped) dishes are exempt and eligible for early reuse. Re-running for a week that is already ACTIVE rebuilds it in place and never sets PREVIOUS = ACTIVE.

---

## Friday Pipeline

```
6:00 AM → Developer  (1st Friday of month only)
8:00 AM → Critic     (reads previous week's ratings → updates Preferences.md + writes Lessons Learned)
4:30 PM → Archivist  (copies finished week's files to Archive/)
5:00 PM → Chef       (builds new menu, recipe files, shopping list, dashboard; rolls Current_Week.md)
5–7:30 PM → Sean's correction window (menu tweaks land in the ledger before scheduling)
7:30 PM → Scheduler  (creates calendar dinner events with Google Drive recipe links)
7:45 PM → Scribe     (refreshes README + writes .scribe_commit_msg.txt trigger)
8:15 PM → github_sync.ps1  (Windows Task Scheduler — picks up trigger, commits, pushes)
~9 PM   → Manager    (daily check — verifies the whole pipeline ran)
```

---

## Rating Flow

```
Sunday 7 PM   → Surveyor populates Rate_This_Week.md with the PREVIOUS week's cooked,
                unrated dishes (skipping RATED and DROPPED ones), refreshes the standalone
                survey artifact, and sends a Monday 9 AM email (Google Calendar) + ntfy nudge.
Anytime       → Sean opens the rating artifact → rates dishes → saves.
                Artifacts can't write local files, so ratings persist to a Google Drive
                doc (the Cookbook Drive folder) rather than to disk directly.
Wednesday     → Manager sends a second ntfy nudge if the week is still unrated.
Friday 8 AM   → Critic collects the ratings from Drive → appends to Recipe_Ratings.md
                → updates Preferences.md → writes Lessons Learned for the Chef.
```

Ratings feed the menu roughly two Fridays later. Notifications go out via **both** Google Calendar (email) **and** ntfy.

---

## GitHub Sync Architecture

The Cowork sandbox has no outbound internet, so The Scribe never runs `git` directly. Instead:

1. **The Scribe** (Friday 7:45 PM) refreshes this README and writes a one-line commit message to `System/.scribe_commit_msg.txt`.
2. **`System/github_sync.ps1`** runs on the **Windows host** via Task Scheduler ("Royal Kitchen - GitHub Sync") at **8:15 PM**. It checks for the trigger file, and if present: syncs files, scrubs secrets from copied task prompts, commits with that message, and pushes to **`ARPnemesis/seans-kitchen`** using the GitHub App key in `System/`.
3. After a successful push, the PS1 deletes the trigger file so the next run is a clean no-op until The Scribe writes a new one.

Auth is handled entirely host-side by the GitHub App private key — The Scribe never needs it.

---

## Artifacts (live dashboards)

- **`kings-table-kitchen-dashboard`** — menu, macro scoreboard, and one-click Instacart cart build. Refreshed each Friday by the Chef. The cart assumes Sean owns nothing and includes every ingredient, then subtracts whatever is marked owned in the Inventory app (this week: ~42 items after 18 inventory subtractions from a 60-item union). Includes a "Save menu changes" bridge button (writes a `Menu_Adjustment_*` doc to Drive for the Manager's daily reconcile) and a "Generate a fresh menu now" button that triggers the Chef via `runScheduledTask("weekly-kings-menu")`.
- **`kings-table-inventory`** — Drive-backed checklist where Sean marks what he already has on hand; it saves a `Kitchen_Inventory` doc to the Cookbook Drive folder, which the dashboard cart reads at build time.
- **`kings-table-rate-this-week`** — standalone weekly rating survey, refreshed each Sunday by the Surveyor with the week just finished.

---

## Repository Structure

```
seans-kitchen/
│
├── README.md                        ← this file (refreshed weekly by The Scribe)
├── Menu_Week_of_*.md                ← the week's menu (authoritative week set by Current_Week.md)
├── Shopping_List_Week_of_*.md       ← the week's shopping list
├── Rate_This_Week.md                ← rating form (Surveyor-owned; created Sunday, read Friday)
├── Weekly_Staples.md                ← standing pantry staples
├── Recipes/                         ← one .md per dish
├── Archive/                         ← finished weeks, filed by the Archivist
└── System/                          ← pointers, logs, scripts, charters
    ├── Current_Week.md              ← single source of truth for the active/previous week
    ├── Kitchen_Log.md               ← briefing board (every task logs here)
    ├── Preferences.md               ← evolving taste profile
    ├── Recipe_Ratings.md            ← rating history
    ├── github_sync.ps1              ← host-side commit + push
    └── .scribe_commit_msg.txt       ← commit-message trigger (written by the Scribe)
```

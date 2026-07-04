# Sean's Royal Kitchen

*Automated weekly meal planning system · Established June 2026*

---

## What This Is

A fully automated pipeline that runs every Friday and builds a personalized 5-dinner menu, writes individual recipe files, creates a shopping list, schedules dinner calendar events with Google Drive recipe links, refreshes a live dashboard, and version-controls everything to GitHub — all informed by Sean's taste ratings and an evolving taste profile.

---

## Current Week

**Week of 2026-07-06** (Mon Jul 6 – Sun Jul 12) — 5 dishes, avg ~42.6g protein / ~520 cal

*Authoritative week and dish lineup come from `System/Current_Week.md` (`ACTIVE_WEEK`). The on-disk menu file is `Menu_Week_of_2026-07-06.md`, built on-cycle by The Chef for the week of 2026-07-06.*

| Dish | Style | Night | Protein | Calories | Time |
|------|-------|-------|---------|----------|------|
| Garlic Butter Chicken Thighs & Broccoli | American Classic | Mon 07/06 | ~42g | ~440 | 25 min |
| Southwest Turkey & Black Bean Stuffed Sweet Potatoes | Southwest/Tex-Mex | Tue 07/07 | ~40g | ~540 | 30 min |
| Cuban Mojo Pork Chops with Black Beans & Rice | Cuban/Latin | Wed 07/08 | ~42g | ~520 | 30 min |
| Lemon-Garlic Butter Scallops with Asparagus & Orzo | Coastal/Mediterranean | Thu 07/09 | ~45g | ~520 | 25 min |
| Peruvian Beef Stir-Fry (Lomo Saltado) | Peruvian | Sat 07/11 (weekend) | ~44g | ~580 | 40 min |

*Four brand-new dishes plus a returning champion — **Garlic Butter Chicken Thighs & Broccoli** (5★, last cooked the week of June 8) comes back on The Critic's recommendation, well clear of the recycle window. The other four are all-new, swinging the cuisine spread toward Southwest, Cuban, coastal Mediterranean, and Peruvian after three straight weeks of Thai, Korean, Cajun, Mexican, and American comfort. Every dish clears 40g protein and stays under 600 calories. Full recipes in `Recipes/`; each also has a Google Doc in the Drive Recipes folder for mobile access.*

**Previously cooked (week of 2026-06-29, now finished and in the rating queue):** Thai Basil Chicken Bowls, Korean Beef Bulgogi Bowls, Maple-Dijon Glazed Salmon with Roasted Brussels Sprouts, Cajun Honey-Butter Shrimp Bowls, Mississippi Pot Roast. This is the current rating queue (per `Current_Week.md` `PREVIOUS_DISHES`); the Surveyor seeds it for rating and the Critic processes it the following Friday.

---

## The Team

| Employee | Task ID | Schedule | Role |
|----------|---------|----------|------|
| 👑 The Kitchen Manager | `the-manager` | Daily ~9 PM | Checks and balances on all employees. Approves mid-tier changes. Escalates failures to Sean. Sends Wednesday rating nudge if the week is still unrated. |
| 🔧 The Developer | `kitchen-developer` | 1st Friday 6 AM | Monthly system review. Auto-fixes minor issues. Proposes change requests. |
| 🎭 The Critic | `meal-critic-weekly` | Friday 8 AM | Reads the previous week's ratings, updates the taste profile, writes Lessons Learned for the Chef. |
| 🗄️ The Archivist | `kitchen-archivist` | Friday 4:30 PM | Archives the finished week's files to `Archive/` before the Chef overwrites them. |
| 🍳 The Chef | `weekly-kings-menu` | Friday 5 PM | Builds the coming week's 5 brand-new dishes, recipe files, and shopping list; rolls the `Current_Week.md` pointer; refreshes the dashboard. |
| 📅 The Scheduler | `kitchen-scheduler` | Friday 5:30 PM | Assigns the active dishes to free evenings; creates calendar events with Google Drive recipe links. |
| 📜 The Scribe | `kitchen-scribe` | Friday 5:45 PM | Refreshes this README and drops the commit-message trigger for the Windows host GitHub sync. |
| 📬 The Surveyor | `meal-surveyor` | Sunday 7 PM | Owns `Rate_This_Week.md` (seeds it with the previous week's dishes); refreshes the standalone rating artifact; sends Monday 9 AM email + ntfy nudge. |

---

## Single Source of Truth — `System/Current_Week.md`

Every task reads `Current_Week.md` to decide which week it operates on — **never** by guessing from "the most recent `Menu_Week_of_*.md`" (that guessing caused off-cycle drift). It holds:

- **`ACTIVE_WEEK` / `ACTIVE_DISHES`** — the Monday and dishes currently being cooked and scheduled.
- **`PREVIOUS_WEEK` / `PREVIOUS_DISHES`** — the week most recently finished, which the Surveyor asks Sean to rate.

Each Friday the Chef rolls the ledger forward: the old ACTIVE becomes PREVIOUS, and the newly built week becomes ACTIVE. Every week is a fresh slate of 5 dishes — there is no carryover, and no dish from the last 3 weeks repeats (except a Critic-recommended 4+ week-old favorite). Re-running for a week that is already ACTIVE rebuilds it in place and never sets PREVIOUS = ACTIVE.

---

## Friday Pipeline

```
6:00 AM → Developer  (1st Friday of month only)
~9 PM   → Manager    (daily check — verifies Developer ran if 1st Friday)
8:00 AM → Critic     (reads previous week's ratings → updates Preferences.md + writes Lessons Learned)
4:30 PM → Archivist  (copies finished week's files to Archive/)
5:00 PM → Chef       (builds new menu, recipe files, shopping list, dashboard; rolls Current_Week.md)
5:30 PM → Scheduler  (creates calendar dinner events with Google Drive recipe links)
5:45 PM → Scribe     (refreshes README + writes .scribe_commit_msg.txt trigger)
6:15 PM → github_sync.ps1  (Windows Task Scheduler — picks up trigger, syncs files, commits, pushes)
```

---

## Rating Flow

```
Sunday 7 PM   → Surveyor populates Rate_This_Week.md with the PREVIOUS week's dishes,
                refreshes the standalone survey artifact, and sends a Monday 9 AM
                email (Google Calendar) + ntfy nudge.
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

1. **The Scribe** (5:45 PM) refreshes this README and writes a one-line commit message to `System/.scribe_commit_msg.txt`.
2. **`System/github_sync.ps1`** runs on the **Windows host** via Task Scheduler ("Royal Kitchen - GitHub Sync") at **6:15 PM**. It checks for the trigger file, and if present: syncs files, scrubs secrets from copied task prompts, commits with that message, and pushes to **`ARPnemesis/seans-kitchen`** using the GitHub App key in `System/`.
3. After a successful push, the PS1 deletes the trigger file so the next run is a clean no-op until The Scribe writes a new one.

Auth is handled entirely host-side by the GitHub App private key — The Scribe never needs it.

---

## Artifacts (live dashboards)

- **`kings-table-kitchen-dashboard`** — menu, macro scoreboard, and one-click Instacart cart build. Refreshed each Friday by the Chef. The cart assumes Sean owns nothing and includes every ingredient, then subtracts whatever is marked owned in the Inventory app. The "Generate a fresh menu now" button triggers the Chef via `runScheduledTask("weekly-kings-menu")`.
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

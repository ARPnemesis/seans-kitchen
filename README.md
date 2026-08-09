# Sean's Royal Kitchen

*Automated weekly meal planning system · Established June 2026*

---

## What This Is

A fully automated pipeline that runs every Friday and builds a personalized 5-dinner menu, writes individual recipe files, creates a shopping list, schedules dinner calendar events with Google Drive recipe links, refreshes a live dashboard, and version-controls everything to GitHub — all informed by Sean's taste ratings and an evolving taste profile.

---

## Current Week

**Week of 2026-08-10** (Mon Aug 10 – Sun Aug 16)

*The authoritative week and dish slate come from `System/Current_Week.md` (`ACTIVE_WEEK` / `ACTIVE_DISHES`), including its per-dish status annotations. The menu file is `Menu_Week_of_2026-08-10.md`. **Night assignments below are derived from live Google Calendar 🍽️ events, read at the moment this file was written (2026-08-08 8:15 PM) — they are not stored in the ledger.** As of this writing the slate carries **no annotations**: all five dishes stand as planned, none dropped.*

| Dish | Style | Night | Protein | Calories | Notes |
|------|-------|-------|---------|----------|-------|
| Cuban Picadillo with Black Beans & Rice | Cuban | Tue 08/11, 5:45 PM | ~42g | ~575 | New · 35 min · early start to clear the 7:00 PM change-request review |
| Korean Braised Chicken & Potatoes (Dak-Dori-Tang) | Korean | Wed 08/12, 6:30 PM | ~44g | ~570 | New · 45 min · braise, better on day two |
| Baked Rigatoni with Italian Sausage & Ricotta | Italian | Thu 08/13, 6:30 PM | ~46g | ~595 | New · 50 min (20 active) · best-reheating dish on the board |
| Cajun Honey-Butter Shrimp Bowls | Cajun/Creole | Sat 08/15, 7:00 PM | ~40g | ~525 | 25 min · **best eaten fresh** — weekend for freshness, not effort |
| Mississippi Pot Roast | American Comfort | Sun 08/16, 7:00 PM | ~40g | ~480 | 8 hrs slow cooker · **start it by ~10:30 AM** · Critic's top recycle pick (5★ ×2) |

**Also on the calendar this week:** Mon 08/10 6:30 PM holds **Chipotle Chicken Tinga Rice Bowls**, a dish from the week of 08-03 that Sean moved forward. It is a live calendar event and a real dinner, but it is not part of the 08-10 slate in the ledger; the Kitchen Manager reconciles that on its next daily pass. **Fri 08/14 is intentionally empty** — see the pipeline note below.

Average **~42.4g protein · ~549 calories** per serving. Every dish clears the 35g protein floor and sits under the 600-calorie ceiling. Five distinct proteins (ground beef, chicken thighs, Italian sausage, beef chuck, shrimp) and five distinct cuisines. Zero Greek yogurt; zero quick-sear proteins on a weeknight.

Two entries are deliberate, sanctioned exceptions to the no-repeat rule: **Mississippi Pot Roast** is a Critic-flagged recycle (5★ twice, last served 07-13, explicitly its top pick for this week), and **Cajun Honey-Butter Shrimp Bowls** was dropped un-cooked on 06-29, so the no-repeat window never applied to it. The other three are new to the kitchen.

**Built on the reheat criterion.** Two-portion meals for one adult mean next-day lunch is most of the diet, so the week is one oven bake, two braises and one slow-cooked roast — and **every recipe card now names its reheat method by hand**, which the Critic identified as the cheapest available 4★→5★ upgrade.

### Previously cooked — week of 2026-08-03

Chipotle Chicken Tinga Rice Bowls · Thai Red Curry Ground Turkey with Green Beans · Smothered Pork Tenderloin Medallions with Mushroom Gravy · Moroccan Ground Lamb & Chickpea Skillet with Couscous · Hungarian Beef Goulash with Buttered Egg Noodles

This is the week the Surveyor asks Sean to rate, the Critic scores, and the Archivist files.

---

## The Team

Eight scheduled tasks, all running on Denver time.

| Name | Schedule | Role |
|------|----------|------|
| **The Critic** | Fri 12:00 PM | Reads the week's ratings, updates the taste profile, writes `Lessons_Learned_*.md` |
| **The Archivist** | Fri 4:30 PM | Archives the finished week before the Chef overwrites it; resets the rating form; trims the Kitchen Log |
| **The Chef** | Fri 5:00 PM | Builds the new menu, five recipe files, the shopping list; refreshes the dashboard; rolls `Current_Week.md` |
| **The Scheduler** | Fri 7:30 PM | Assigns dishes to free evenings, creates 🍽️ calendar events with Drive recipe links |
| **The Scribe** | Fri 7:45 PM | Refreshes this README and drops the commit trigger for the host GitHub sync |
| **The Surveyor** | Sun 7:00 PM | Seeds the rating form and creates the Monday 9 AM reminder to rate the week's meals |
| **The Kitchen Manager** | Daily 9:00 PM | Reconciles the ledger against the calendar and dashboard, peer-reviews every task's output, escalates to Sean |
| **The Developer** | 1st & 3rd Wed 11:00 AM | Bi-weekly system review — auto-fixes minor issues, escalates major ones as Change Requests |

The Developer moved off Friday and onto a bi-weekly Wednesday on 2026-08-07, deliberately: its prompt changes now land about two days before the pipeline executes them instead of about one hour.

---

## The Friday Pipeline

```
12:00 PM  THE CRITIC       reads ratings → Lessons Learned
             ↓
 4:30 PM  THE ARCHIVIST    archives last week, resets the rating form
             ↓
 5:00 PM  THE CHEF         builds the menu, recipes, shopping list, dashboard
             ↓
          ┌──────────────────────────────────────────────┐
          │  5:00 – 7:30 PM   SEAN'S CORRECTION WINDOW   │
          │  Review the menu; deselect a dish and the    │
          │  dashboard writes a Menu_Adjustment doc the  │
          │  Scheduler reads before booking anything.    │
          └──────────────────────────────────────────────┘
             ↓
 7:30 PM  THE SCHEDULER    books 🍽️ dinner events on free evenings
             ↓
 7:45 PM  THE SCRIBE       refreshes README, drops the commit trigger
             ↓
 8:15 PM  HOST PS1         commits + pushes everything to GitHub
             ↓
 9:00 PM  THE MANAGER      verifies the whole pipeline the same evening
```

**Friday evening is deliberately kept free as an overflow slot.** This is a design decision Sean made on 2026-08-07 (CR-C), not a scheduling gap left by the pipeline day. It exists so that any dish pushed off a weeknight has a guaranteed landing spot. The menu is built around Monday–Thursday plus the weekend and does not assume a Friday cooking slot — so an empty Friday is correct, and a booked Friday usually means Sean moved something there himself. Neither is an error.

**Day assignments are not stored anywhere.** `Current_Week.md` is authoritative for *which* dishes belong to a week and nothing else. *Which night* a dish is cooked is derived from live Google Calendar events, every time, by every task — Sean edits the calendar directly, sometimes minutes after a task has read it, and four consecutive weeks of drift showed the slate was right every time while only the days went stale.

---

## Sync Architecture

The scheduled tasks run in a sandbox with no outbound internet, so nothing in the pipeline can reach GitHub directly. The push is a two-stage handoff:

```
THE SCRIBE (Fri 7:45 PM, sandbox)
   writes  README.md
   writes  System/.scribe_commit_msg.txt   ← the trigger
             ↓
"Royal Kitchen - GitHub Sync" (Windows Task Scheduler, Fri 8:15 PM)
   runs    System/github_sync.ps1
   reads   the trigger file, uses it as the commit message
   auths   as a GitHub App via System/*.private-key.pem → JWT → install token
   clones  ARPnemesis/seans-kitchen, syncs files, commits, pushes
   logs    System/.github_sync_log.txt
```

If no trigger file is present the script logs `No trigger file found - sync skipped` and does nothing — so a Scribe run that never happened cannot produce a misleading commit. A trigger dropped after 8:15 PM is picked up on the next run rather than lost.

---

## Repository Layout

```
Menu_Week_of_*.md            weekly menus
Shopping_List_Week_of_*.md   weekly shopping lists (built for a hand-assembled King Soopers pickup cart)
Lessons_Learned_Week_of_*.md the Critic's weekly analysis
Recipes/                     the recipe library
Archive/                     completed weeks, filed by the Archivist
Rate_This_Week.md            the rating form, reset weekly
How_This_Kitchen_Works.md    the plain-language overview
System/
  Current_Week.md            the ledger — single source of truth for the active slate
  Kitchen_Log.md             the shared briefing board; every task reads it and writes to it
  Preferences.md             Sean's taste profile and standing requests
  Recipe_Ratings.md          every dish ever rated
  Kitchen_Manager_Charter.md roles, authorities, escalation chain
  Change_Requests/           major changes awaiting or holding Sean's sign-off
  Kitchen_Log_Archive/       trimmed log history
  *.ps1                      host-side scripts (GitHub sync, ntfy notifications)
```

---

*Maintained by The Scribe. Last refreshed 2026-08-08.*

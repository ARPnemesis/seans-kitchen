# Sean's Royal Kitchen

*Automated weekly meal planning system · Established June 2026*

---

## What This Is

A fully automated pipeline that runs every Friday and builds a personalized 5-dinner menu, writes individual recipe files, creates a shopping list, schedules dinner calendar events with Google Drive recipe links, refreshes a live dashboard, and version-controls everything to GitHub — all informed by Sean's taste ratings and an evolving taste profile.

---

## Current Week

**Week of 2026-09-07** (Mon Sep 7 – Sun Sep 13)

*The authoritative week and dish slate come from `System/Current_Week.md` (`ACTIVE_WEEK` / `ACTIVE_DISHES`). The menu file is `Menu_Week_of_2026-09-07.md`. **Night assignments below are derived from live Google Calendar 🍽️ events, read at the moment this file was written (2026-09-04, ~7:57 PM Denver) — they are not stored in the ledger.***

| Dish | Style | Night | Protein | Calories | Notes |
|------|-------|-------|---------|----------|-------|
| Sesame-Ginger Teriyaki Salmon with Broccoli & Rice | Japanese-inspired | Mon 09/07, 6:30 PM | ~40g | ~520 | 25 min · new cuisine · cornstarch-dredge crust, front-loaded for perishability |
| Nigerian-Inspired Suya-Spiced Chicken Thighs with Peanut Dipping Sauce | West African | Tue 09/08, 6:30 PM | ~42g | ~540 | 35 min · new cuisine |
| Turkish Ground Turkey Kofta Bowls with Tahini Drizzle | Turkish | Wed 09/09, 6:30 PM | ~38g | ~510 | 30 min · new cuisine · zero Greek yogurt, tahini does the sauce work |
| Korean Beef Bulgogi Bowls | Korean | Thu 09/10, 6:30 PM | ~40g | ~560 | 30 min · recycle, Critic-flagged 4–5★ favorite |
| Puerto Rican Pernil-Style Braised Pork Shoulder with Rice & Pigeon Peas | Puerto Rican | Sun 09/13, 7:00 PM | ~48g | ~590 | 2 hr, mostly hands-off · early reuse of a never-cooked dish (built 08-28, deselected before it was ever cooked, so no no-repeat penalty), recipe card reused unchanged · card says FREEZE ON ARRIVAL |

**Fri 09/11 and Sat 09/12 are both empty.** Friday is the standing overflow slot (see the pipeline note below) and stays empty by design; Saturday is empty because this week has only one weekend dish.

All five dishes are booked this week — no corrections were made in Friday's window (no `Menu_Adjustment_2026-09-07` doc exists), so the full Chef-built slate went straight to the calendar. Average across all five: **~42g protein · ~544 calories** per serving, every dish clearing the 35g floor and staying under 600 calories. Five distinct proteins (salmon, chicken thighs, ground turkey, beef, pork shoulder) and five distinct cuisines, three of them brand-new to the kitchen (Nigerian, Turkish, and Puerto Rican's first actual cook).

The only calendar commitment this week is Monday's midday WGU mentor call (12:10–12:25 PM) — no dinner conflict. Sunday the 13th is Dennis's birthday (all-day, no evening block) — noted, not avoided.

**Built on the Critic's 09-04 briefing**, itself written off a strong four-dish week (08-24 averaged 4.5★, with Chicken Karahi and a second serving of Jamaican Jerk Chicken both landing 5★ — both still inside their no-repeat window, so neither is on this board yet). Korean Beef Bulgogi Bowls returns as the Critic's other standing recommendation, and the Puerto Rican pork shoulder finally gets its night after sitting deselected-but-never-cooked for two weeks. The early-reuse pool is now empty again (Hungarian Beef Goulash and Korean Braised Chicken remain twice-declined soft passes, not live candidates).

### Previously cooked — week of 2026-08-31

Salmon Tacos with Mango-Corn Salsa · Hawaiian-Style Turkey Meatballs with Pineapple Fried Rice · Harissa Braised Chicken Thighs with Chickpeas & Couscous · Brazilian Garlic Butter Steak Bowls · Turkey Shepherd's Pie with Cheddar-Chive Mash *(carried in from the week of 08-24, cooked Mon 08-31)*

**Five dishes actually cooked.** Puerto Rican Pernil-Style Braised Pork Shoulder was originally planned for this week but deselected in the correction window before it was ever cooked — `(DROPPED 2026-08-28 — not cooked)` in the ledger — so it doesn't count against this week's slate; it returns cooked in the current week instead. This is the week the Surveyor asks Sean to rate, the Critic scores, and the Archivist files.

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
| **The Surveyor** | Mon 7:00 AM | Seeds the rating form and the reminder to rate the week just finished |
| **The Kitchen Manager** | Daily 9:00 PM | Reconciles the ledger against the calendar and dashboard, peer-reviews every task's output, escalates to Sean |
| **The Developer** | 1st & 3rd Wed 11:00 AM | Bi-weekly system review — auto-fixes minor issues, escalates major ones as Change Requests |

The Developer moved off Friday and onto a bi-weekly Wednesday on 2026-08-07, deliberately: its prompt changes now land about two days before the pipeline executes them instead of about one hour. The Surveyor moved from Sunday evening to Monday morning on 2026-08-19 (CR-H2), so it never surveys a week that is still mid-cook.

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

**The correction window is real, even in a week it goes unused.** This week Sean opened no `Menu_Adjustment` doc, so the Scheduler booked the Chef's full five-dish slate exactly as built. Some weeks he deselects a dish instead — that variance is exactly why the Scribe runs at 7:45 PM rather than alongside the Chef: by the time this README is written, it describes the week as Sean actually left it.

**Friday evening is deliberately kept free as an overflow slot.** This is a design decision Sean made on 2026-08-07 (CR-C), not a scheduling gap left by the pipeline day. It exists so that any dish pushed off a weeknight has a guaranteed landing spot. The menu is built around Monday–Thursday plus the weekend and does not assume a Friday cooking slot — so an empty Friday is correct, and a booked Friday usually means Sean moved something there himself. Neither is an error.

**Day assignments are not stored anywhere.** `Current_Week.md` is authoritative for *which* dishes belong to a week and nothing else. *Which night* a dish is cooked — or whether it's cooked at all — is derived from live Google Calendar events, every time, by every task. Sean edits the calendar directly, sometimes minutes after a task has read it, and several consecutive weeks of drift showed the slate was right every time while only the days went stale.

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
  Kitchen_Log.md              the shared briefing board; every task reads it and writes to it
  Preferences.md              Sean's taste profile and standing requests
  Recipe_Ratings.md           every dish ever rated
  Kitchen_Manager_Charter.md  roles, authorities, escalation chain
  Change_Requests/            major changes awaiting or holding Sean's sign-off
  Kitchen_Log_Archive/        trimmed log history
  *.ps1                       host-side scripts (GitHub sync, ntfy notifications)
```

---

*Maintained by The Scribe. Last refreshed 2026-09-04.*

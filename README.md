# Sean's Royal Kitchen

*Automated weekly meal planning system · Established June 2026*

---

## What This Is

A fully automated pipeline that runs every Friday and builds a personalized 5-dinner menu, writes individual recipe files, creates a shopping list, schedules dinner calendar events with Google Drive recipe links, refreshes a live dashboard, and version-controls everything to GitHub — all informed by Sean's taste ratings and an evolving taste profile.

---

## Current Week

**Week of 2026-08-24** (Mon Aug 24 – Sun Aug 30)

*The authoritative week and dish slate come from `System/Current_Week.md` (`ACTIVE_WEEK` / `ACTIVE_DISHES`), including its per-dish status annotations. The menu file is `Menu_Week_of_2026-08-24.md`. **Night assignments below are derived from live Google Calendar 🍽️ events, read at the moment this file was written (2026-08-21 7:55 PM) — they are not stored in the ledger.** The slate carries **no annotations**: all five dishes stand as planned, none dropped, none carried in.*

| Dish | Style | Night | Protein | Calories | Notes |
|------|-------|-------|---------|----------|-------|
| Vietnamese Lemongrass Pork Meatball Bowls | Vietnamese | Mon 08/24, 6:30 PM | ~42g | ~560 | 35 min · 2 meals · **clean re-run with real lemongrass** · ground pork, front-half |
| Cajun Dirty Rice Skillet with Ground Beef & Andouille | Cajun / Creole | Tue 08/25, 6:30 PM | ~44g | ~590 | New · 40 min · **3 meals** · one pot, zero dairy · highest-yield weeknight dish |
| Jamaican Jerk Chicken Thighs with Coconut Rice & Black Beans | Caribbean | Wed 08/26, 6:30 PM | ~48g | ~585 | 40 min · 2 meals · **the one recycle** — 5★, window clears exactly this week |
| Chicken Karahi with Basmati & Naan | Indian / Pakistani | Thu 08/27, 6:30 PM | ~50g | ~575 | New · 40 min · 2 meals · highest protein on the board · no yogurt, no cream |
| Turkey Shepherd's Pie with Cheddar-Chive Mash | British comfort | Sun 08/30, 7:00 PM | ~45g | ~580 | New · 55 min · **3 meals** · 🧊 **FREEZE THE TURKEY ON ARRIVAL** |

**Fri 08/28 and Sat 08/29 are empty and that is correct** — Friday is the standing overflow slot (see the pipeline note below), and **Saturday is Ian's birthday**, so the weekend dish was aimed at Sunday deliberately. That is now the seventh consecutive week the weekend dish has landed on Sunday rather than Saturday.

Average **~46g protein · ~578 calories** per serving. Every dish clears the 35g protein floor and sits under the 600-calorie ceiling. Five distinct proteins (ground pork, ground beef + andouille, chicken thighs, chicken breast, ground turkey) and five distinct cuisines. **Total yield ~12 meals** — 5 dinners plus roughly 7 lunches, with two dishes making three meals each.

**Built on the reheat criterion, and on three explicit instructions from the Critic.** Four of five rating notes last week led with the reheat before mentioning taste, so **every recipe card names its reheat method** and **prints its yield in meals rather than servings**. The third instruction is a roster decision: **shrimp is off the board** — after a second independent reheat complaint Sean generalized it himself (*"I think shrimp is a no for reheats altogether"*), and since next-day lunch is most of the diet, a protein that can't survive a reheat is structurally low-value here.

**Two entries are not brand new, and both are sanctioned.** The **jerk chicken** is the one permitted no-repeat exception — the Critic's explicit top pick, 5★, the strongest single note in eleven weeks, and its four-week window clears exactly now. The **lemongrass pork meatballs** are a *re-serve of a dish rated with a substitution*: the 4★ was scored on a build where the lemongrass never made the cart and lemon pepper stood in, which is a verdict on a shopping list rather than a recipe. This week the shopping list names fresh lemongrass in capitals as an identity ingredient and rules lemon pepper out by name.

### Previously cooked — week of 2026-08-17

Philly Cheesesteak Hoagies · Harissa-Honey Salmon with Lemon-Herb Rice & Blistered Green Beans · Smoky Chipotle Pork & Black Bean Chili · Filipino Chicken Adobo with Garlic Rice · Mongolian Beef with Jasmine Rice & Charred Scallions

**Five cooked dishes, no annotations.** The chili was moved by Sean from Thursday onto Friday 08-21 — a day reassignment inside its own week, not a drop — which is the sixth consecutive time a dish leaving its slot turned out to be a move rather than a skip. This is the week the Surveyor asks Sean to rate, the Critic scores, and the Archivist files.

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

**The correction window is real and it gets used.** This week Sean opened it, reviewed the slate at 7:28 PM, and changed nothing — the adjustment doc lists all five dishes as kept. A week earlier he cut two dishes inside the same window. That variance is exactly why the Scribe runs at 7:45 PM rather than alongside the Chef: by the time this README is written, it describes the week as Sean actually left it.

**Friday evening is deliberately kept free as an overflow slot.** This is a design decision Sean made on 2026-08-07 (CR-C), not a scheduling gap left by the pipeline day. It exists so that any dish pushed off a weeknight has a guaranteed landing spot. The menu is built around Monday–Thursday plus the weekend and does not assume a Friday cooking slot — so an empty Friday is correct, and a booked Friday usually means Sean moved something there himself. Neither is an error. It caught its first live dish on 2026-08-20, when a chili pushed off Thursday landed on the empty Friday instead of being dropped.

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

*Maintained by The Scribe. Last refreshed 2026-08-21.*

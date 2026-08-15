# Sean's Royal Kitchen

*Automated weekly meal planning system · Established June 2026*

---

## What This Is

A fully automated pipeline that runs every Friday and builds a personalized 5-dinner menu, writes individual recipe files, creates a shopping list, schedules dinner calendar events with Google Drive recipe links, refreshes a live dashboard, and version-controls everything to GitHub — all informed by Sean's taste ratings and an evolving taste profile.

---

## Current Week

**Week of 2026-08-17** (Mon Aug 17 – Sun Aug 23)

*The authoritative week and dish slate come from `System/Current_Week.md` (`ACTIVE_WEEK` / `ACTIVE_DISHES`), including its per-dish status annotations. The menu file is `Menu_Week_of_2026-08-17.md`. **Night assignments below are derived from live Google Calendar 🍽️ events, read at the moment this file was written (2026-08-14 7:55 PM) — they are not stored in the ledger.** The slate carries **no annotations**: all five dishes stand as planned, none dropped, none carried in.*

| Dish | Style | Night | Protein | Calories | Notes |
|------|-------|-------|---------|----------|-------|
| Harissa-Honey Salmon with Lemon-Herb Rice & Blistered Green Beans | North African | Mon 08/17, 6:30 PM | ~44g | ~575 | New · 30 min · front-loaded — most perishable protein on the board |
| Filipino Chicken Adobo with Garlic Rice | Filipino *(new cuisine)* | Tue 08/18, 6:30 PM | ~47g | ~580 | New · 40 min (10 active) · best reheat on the board · almost entirely pantry |
| Philly Cheesesteak Hoagies | American comfort | Wed 08/19, 6:30 PM | ~48g | ~625 | 30 min · **the standing hoagie request, delivered** · peppers salted before the pan |
| Smoky Chipotle Pork & Black Bean Chili | Tex-Mex / American | Thu 08/20, 6:30 PM | ~46g | ~545 | New · 40 min · first chili in nine weeks · makes more than two servings |
| Mongolian Beef with Jasmine Rice & Charred Scallions | Chinese *(new cuisine)* | Sun 08/23, 7:00 PM | ~45g | ~590 | New · 30 min · 🧊 **FREEZE THE STEAK ON ARRIVAL** — flank slices cleaner semi-frozen |

**Fri 08/21 and Sat 08/22 are empty and that is correct** — Friday is the standing overflow slot (see the pipeline note below), and the week was built around Monday–Thursday plus one weekend night.

Average **~46g protein · ~583 calories** per serving — the highest weekly protein average on record. Every dish clears the 35g protein floor and sits under the 600-calorie ceiling. Five distinct proteins (salmon, chicken thighs, shaved beef sirloin, ground pork, flank steak) and five distinct cuisines, **three of them new to the kitchen**. Zero Greek yogurt. Nothing on the board takes more than 40 minutes.

**This week was revised after it was built.** The Chef's 5:20 PM slate carried two long braises — a Korean braised chicken and a Hungarian goulash — and Sean cut both at 5:30 PM inside the correction window. Rather than treat that as two dish rejections, the Chef read it as a verdict on the shape of the week and replaced them with two quick, saucy dishes: **there is no 2-hour weekend project on this menu at all.** The revision also made the week materially easier to shop.

**Built on the reheat criterion.** Two-portion meals for one adult mean next-day lunch is most of the diet, so four of the five dishes are saucy enough to survive a night in the fridge, and **every recipe card names its reheat method by hand**. Two further rules from the Critic are applied five-for-five: **every specialty ingredient names a sanctioned fallback** on its card, and **a vegetable-ratio ceiling** is printed in bold (green beans capped at 8 oz after 10 oz "absolutely dominated" the Thai curry).

### Previously cooked — week of 2026-08-10

Chipotle Chicken Tinga Rice Bowls *(carried in from 08-03)* · Cuban Picadillo with Black Beans & Rice · Baked Rigatoni with Italian Sausage & Ricotta · Mississippi Pot Roast · Cajun Honey-Butter Shrimp Bowls

**Five cooked dishes, not six.** The Korean Braised Chicken & Potatoes was on the 08-10 plan but Sean removed it on 08-09 for slate size and it was never cooked, so it is annotated `DROPPED` in the ledger and is excluded from rating. This is the week the Surveyor asks Sean to rate, the Critic scores, and the Archivist files.

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

**The correction window is real and it gets used.** This week Sean rewrote a third of the menu inside it, at 5:30 PM — which is exactly why the Scribe runs at 7:45 PM rather than alongside the Chef. By the time this README is written, it describes the week as Sean actually left it.

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

*Maintained by The Scribe. Last refreshed 2026-08-14.*

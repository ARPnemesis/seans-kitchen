# Sean's Royal Kitchen

*Automated weekly meal planning system · Established June 2026*

---

## What This Is

A fully automated pipeline that runs every Friday and builds a personalized 5-dinner menu, writes individual recipe files, creates a shopping list, schedules dinner calendar events with Google Drive recipe links, refreshes a live dashboard, and version-controls everything to GitHub — all informed by Sean's taste ratings and an evolving taste profile.

---

## Current Week

**Week of 2026-08-31** (Mon Aug 31 – Sun Sep 6)

*The authoritative week and dish slate come from `System/Current_Week.md` (`ACTIVE_WEEK` / `ACTIVE_DISHES`). The menu file is `Menu_Week_of_2026-08-31.md`. **Night assignments below are derived from live Google Calendar 🍽️ events, read at the moment this file was written (2026-08-28, ~7:56 PM Denver) — they are not stored in the ledger.***

| Dish | Style | Night | Protein | Calories | Notes |
|------|-------|-------|---------|----------|-------|
| Salmon Tacos with Mango-Corn Salsa | Coastal Latin | Mon 08/31, 6:30 PM | ~38g | ~540 | 25 min · recycled 5★ favorite, salmon's fifth winning prep style |
| Hawaiian-Style Turkey Meatballs with Pineapple Fried Rice | Hawaiian / Pacific | Tue 09/01, 6:30 PM | ~38g | ~520 | 30 min · new cuisine · front-half perishable |
| Harissa Braised Chicken Thighs with Chickpeas & Couscous | North African | Wed 09/02, 6:30 PM | ~42g | ~560 | 35 min · Critic-directed repeat, new braised format |
| Brazilian Garlic Butter Steak Bowls | Brazilian | Thu 09/03, 6:30 PM | ~40g | ~580 | 25 min · new cuisine, trending format |
| Puerto Rican Pernil-Style Braised Pork Shoulder with Rice & Pigeon Peas | Puerto Rican | *not booked this week* | ~48g | ~590 | Sean deselected it in Friday's correction window (`Menu_Adjustment_2026-08-31`); no calendar event exists for it Mon 08/31–Sun 09/06. Still listed in `Current_Week.md`'s `ACTIVE_DISHES` pending the Kitchen Manager's next reconciliation pass. |

**Fri 09/04, Sat 09/05 and Sun 09/06 are all empty.** Friday is the standing overflow slot (see the pipeline note below) and stays empty by design. Saturday and Sunday are empty because this week has no weekend dish — the pork shoulder was cut in the correction window, so nothing was booked in its place.

Average across the four dishes actually cooking this week: **~39.5g protein · ~550 calories** per serving. All five originally-planned dishes clear the 35g protein floor and stay under 600 calories. Four distinct proteins on the calendar this week (salmon, ground turkey, chicken thighs, steak) and four distinct cuisines, two of them brand-new to the kitchen (Brazilian, Hawaiian/Pacific).

**Built on the Critic's 08-28 12:04 briefing**, itself written off the kitchen's best full week on record — week 08-17 closed at 4.8★ average, five for five "cook again," five for five "easier than expected." Harissa/North African and braised pork are both now 2-for-2 at 5★, so harissa returns in a new braised format and pork was slated to return as a weekend braise before Sean cut it. Cuisine expansion keeps paying off (Filipino and Chinese both landed clean last week), so two more new cuisines — Brazilian and Hawaiian/Pacific — joined the board. The early-reuse pool stays empty (Hungarian Beef Goulash and Korean Braised Chicken remain twice-declined soft passes, not live candidates).

### Previously cooked — week of 2026-08-24

Vietnamese Lemongrass Pork Meatball Bowls · Cajun Dirty Rice Skillet with Ground Beef & Andouille · Jamaican Jerk Chicken Thighs with Coconut Rice & Black Beans · Chicken Karahi with Basmati & Naan · Turkey Shepherd's Pie with Cheddar-Chive Mash

**Five cooked dishes, no annotations.** Two dishes crossed this week's roll boundary and cooked after the Chef's Friday build: the Chicken Karahi (Sean's own day-move from Thursday to Friday 08-28, one hour after tonight's build) and the Turkey Shepherd's Pie (Sunday 08-30). Their absence from this week's calendar is not evidence either was skipped — both are confirmed cooked-or-cooking as of the roll. This is the week the Surveyor asks Sean to rate, the Critic scores, and the Archivist files.

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

**The correction window is real and it gets used.** This week Sean opened it and deselected the weekend pork braise, so the Scheduler booked only the four remaining weeknight dishes — nothing was substituted in its place. A week earlier he reviewed the slate and changed nothing. That variance is exactly why the Scribe runs at 7:45 PM rather than alongside the Chef: by the time this README is written, it describes the week as Sean actually left it.

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
  Kitchen_Log.md             the shared briefing board; every task reads it and writes to it
  Preferences.md             Sean's taste profile and standing requests
  Recipe_Ratings.md          every dish ever rated
  Kitchen_Manager_Charter.md roles, authorities, escalation chain
  Change_Requests/           major changes awaiting or holding Sean's sign-off
  Kitchen_Log_Archive/       trimmed log history
  *.ps1                      host-side scripts (GitHub sync, ntfy notifications)
```

---

*Maintained by The Scribe. Last refreshed 2026-08-28.*

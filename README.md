# Sean's Royal Kitchen 👑

An automated weekly meal-planning system. Every Friday a pipeline of scheduled tasks builds a personalized 5-dinner menu, writes individual recipe files, creates a shopping list, schedules dinner on the calendar with recipe links, and refreshes a live dashboard — all informed by Sean's taste ratings and evolving preferences.

---

## The Team

| Employee | Task ID | Schedule | Role |
|----------|---------|----------|------|
| 👑 The Kitchen Manager | `the-manager` | Daily 7 AM | Checks and balances on all employees. Approves mid-tier changes. Escalates failures to Sean. |
| 🔧 The Developer | `kitchen-developer` | 1st Friday 6 AM | Monthly system review. Auto-fixes minor issues. Proposes improvements. |
| 📋 The Critic | `meal-critic-weekly` | Friday 8 AM | Reads meal ratings, updates taste profile, writes Lessons Learned for the Chef. |
| 🗄️ The Archivist | `kitchen-archivist` | Friday 4:30 PM | Archives this week's files before Chef overwrites them. |
| 🍳 The Chef | `weekly-kings-menu` | Friday 5 PM | Builds menu, recipe files, shopping list, resets Carryover, refreshes dashboard. |
| 📅 The Scheduler | `kitchen-scheduler` | Friday 5:30 PM | Assigns dishes to free evenings, creates calendar events with recipe links. |
| 📬 The Surveyor | `meal-surveyor` | Sunday 7 PM | Creates Rate_This_Week.md, emails Sean Monday 9 AM via Google Calendar. |
| 📝 The Scribe | `kitchen-scribe` | Friday 5:45 PM | Prepares files for GitHub sync, writes commit trigger for local PowerShell job. |

---

## Friday Pipeline

```
6:00 AM  → Developer     (1st Friday of month only)
7:00 AM  → Manager       (daily check)
8:00 AM  → Critic        (reads ratings → updates Preferences.md + Lessons Learned)
4:30 PM  → Archivist     (copies week's files to Archive/)
5:00 PM  → Chef          (builds new menu, recipe files, shopping list, dashboard)
5:30 PM  → Scheduler     (creates calendar dinner events with Google Doc recipe links)
5:45 PM  → Scribe        (updates README, writes git commit trigger)
6:15 PM  → github_sync   (Windows Task Scheduler — pushes everything to GitHub)
```

---

## File Structure

```
G:\My Drive\Cookbook\
│
├── README.md                               ← this file (updated weekly by The Scribe)
├── Menu_Week_of_YYYY-MM-DD.md              ← current week's menu
├── Shopping_List_Week_of_YYYY-MM-DD.md     ← current week's shopping list
├── Carryover.md                            ← live checklist of uncooked dishes
├── Rate_This_Week.md                       ← rating form (created Sunday, read Friday)
│
├── Recipes\                                ← permanent recipe library
│   └── [Dish_Name].md
│
├── Archive\                                ← past weeks (read-only)
│   └── Week_YYYY-MM-DD\
│
└── System\                                 ← system intelligence
    ├── Kitchen_Log.md                      ← shared briefing board
    ├── Preferences.md                      ← Sean's evolving taste profile
    ├── Recipe_Ratings.md                   ← full rating history
    ├── Kitchen_Manager_Charter.md
    ├── Sean's_Kitchen_Project.md           ← master reference doc
    └── Change_Requests\
```

---

## Dashboard

The King's Table Kitchen Dashboard is a live artifact in Cowork. It lets Sean:
- Check off dishes he's skipping this week
- Rate each dish (stars, cook-again, difficulty, notes)
- Mark pantry staples he already owns
- Build his Instacart cart in one click

---

*"Run lean, eat like a king."*

Last synced: 2026-06-05 by The Scribe · [github.com/ARPnemesis/seans-kitchen](https://github.com/ARPnemesis/seans-kitchen)

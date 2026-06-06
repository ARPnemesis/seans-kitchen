# Sean's Kitchen — Project Master Reference
*Royal Kitchen Automation System · Established June 2026*

---

## What This Is

A fully automated weekly meal-planning system. Every Friday a pipeline of six scheduled tasks builds a personalized 5-dinner menu, writes individual recipe files, creates a simple shopping list, schedules dinner on the calendar with recipe links, and refreshes a live dashboard — all informed by Sean's taste ratings and evolving preferences.

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
| 📝 The Scribe | `kitchen-scribe` | Friday 5:45 PM | Syncs all kitchen files and task prompts to GitHub (ARPnemesis/seans-kitchen). Version control + README. |

---

## Friday Pipeline (in order)

```
6:00 AM  → Developer     (1st Friday of month only)
7:00 AM  → Manager       (daily check — verifies Developer ran if 1st Friday)
8:00 AM  → Critic        (reads Rate_This_Week.md → updates Preferences.md + writes Lessons Learned)
4:30 PM  → Archivist     (copies week's files to Archive/)
5:00 PM  → Chef          (builds new menu, recipe files, shopping list, dashboard)
5:30 PM  → Scheduler     (creates calendar dinner events with Google Drive recipe links)
5:45 PM  → Scribe        (syncs all files + task code to GitHub, updates README)
```

---

## Rating Flow

```
Sunday 7 PM  → Surveyor creates Rate_This_Week.md + Monday 9 AM calendar email
               (Sean gets a real email from Google Calendar)
Anytime      → Sean opens King's Table Kitchen Dashboard → rates dishes → hits "Submit"
               → sendPrompt → Kitchen Manager saves ratings to Rate_This_Week.md
Friday 8 AM  → Critic reads Rate_This_Week.md → appends to Recipe_Ratings.md → updates Preferences.md
```

---

## File Structure

```
G:\My Drive\Cookbook\
│
├── Menu_Week_of_YYYY-MM-DD.md          ← current week (overwritten each Friday)
├── Shopping_List_Week_of_YYYY-MM-DD.md ← current week (overwritten each Friday)
├── Carryover.md                        ← live checklist (delete dishes as cooked)
├── Rate_This_Week.md                   ← rating form (created Sunday, read Friday)
│
├── Recipes\                            ← PERMANENT library (never deleted)
│   └── [Dish_Name_With_Underscores].md ← one file per recipe, grows every week
│
├── Archive\                            ← past weeks (read-only history)
│   └── Week_YYYY-MM-DD\
│       ├── Menu_Week_of_*.md
│       ├── Shopping_List_Week_of_*.md
│       ├── Lessons_Learned_*.md
│       ├── Rate_This_Week.md
│       └── Archive_Summary.md
│
└── System\                             ← system intelligence (do not delete)
    ├── Kitchen_Manager_Charter.md      ← authority + escalation rules
    ├── Kitchen_Log.md                  ← shared briefing board (all tasks read + write)
    ├── Preferences.md                  ← Sean's taste profile (Critic updates auto-section)
    ├── Recipe_Ratings.md               ← all historical ratings
    ├── Sean's_Kitchen_Project.md       ← this file
    ├── Developer_Report_*.md           ← monthly system health reports
    └── Change_Requests\
        └── Change_Request_YYYY-MM-DD.md
```

---

## Carryover.md Format

One dish per line, standard markdown task syntax:
```
- [ ] Chicken Shawarma Bowl        ← unchecked = not yet cooked → rolls forward
- [x] Mississippi Pot Roast        ← checked = cooked → does NOT roll forward
```
Delete lines you don't want to carry forward. The Chef reads this every Friday.

---

## Dashboard: King's Table Kitchen Dashboard

Live artifact (refreshed every Friday by the Chef):
- 🎛️ Control Panel — manual menu trigger + cart build shortcut
- 📊 Macro Scoreboard — live avg protein + calories across selected dishes
- 🍽️ This Week's Menu — checkboxes to deselect dishes you're skipping
- ⭐ Rate This Week — star ratings, cook-again, difficulty, notes per dish
- 🥫 Pantry Staples — check what you own; excluded from cart
- 🛒 Build Instacart Cart — one click, clears old cart, adds all selected dishes minus pantry

localStorage keys (persist across sessions):
- `kt_pantry_v1` — your pantry checkmarks
- `kt_dishes_off_v1` — deselected dishes
- `kt_ratings_v1` — in-progress ratings (cleared on new week load)

---

## Escalation Chain

```
Auto-implement  →  Kitchen Manager decides (no approval needed)
Mid-tier change →  Kitchen Manager reviews → implements if approved
Major change    →  Kitchen Manager writes proposal → Sean approves via email
```

Sean gets a real email (via Google Calendar event notification) for:
- 🚨 Critical failures (task crashed, no menu found)
- 🔧 Major change requests requiring approval
- 📬 Weekly meal rating reminder (Monday 9 AM)

---

## Sean's Profile (quick reference)

- **Location:** Englewood, CO · King Soopers via Instacart
- **Budget:** ~$100–130/week groceries
- **Schedule:** 5 dinners/week, eats out often
- **Weeknight:** ≤30 min, minimal prep
- **Weekend:** can be involved (slow cooker fine)
- **Proteins:** chicken thighs, ground turkey/beef, eggs weeknights; steak/salmon/shrimp weekends
- **Goal:** high protein (35g+/serving), moderate calories (<600), lean + hearty, global variety

---

*"Run lean, eat like a king."*

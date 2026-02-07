# Weekly Check-in Command

**Command:** `/weekly-checkin`

**Purpose:** Weekly review and planning

**Duration:** ~10-15 minutes

---

## Overview

The weekly check-in helps you:
- Review the week's progress
- Celebrate wins (streaks, goals hit, personal bests)
- Identify what worked and what didn't
- Discover patterns in your data
- Plan next week's focus

---

## Process

### Step 1: Calculate Current Week

```python
from datetime import datetime, timedelta

today = datetime.now()
week_start = today - timedelta(days=today.weekday() + 1)  # Last Sunday
week_end = week_start + timedelta(days=6)  # This Saturday

week_label = f"Week of {week_start.strftime('%B %d')} - {week_end.strftime('%B %d, %Y')}"
```

### Step 2: Launch Weekly Analyst Agent

Call the `weekly-analyst` agent:

```
Task(
    subagent_type="weekly-analyst",
    description="Generate weekly review",
    prompt="""
    Review the user's week and generate insights.

    Process:
    1. Load week's data from databases/habit-tracker.db
    2. Load user goals from user-profile.json
    3. Calculate stats (check-in rate, habit completion, averages)
    4. Identify patterns (day-of-week, correlations)
    5. Celebrate wins (streaks, milestones, achievements)
    6. Identify what worked and what struggled
    7. Generate key insight
    8. Suggest next week's focus (data-driven)

    Create formatted weekly review report.
    Keep under 5 minutes reading time.
    Be honest about struggles, encouraging about wins.
    Base everything on actual data.
    """
)
```

### Step 3: Agent Generates Report

Agent creates comprehensive weekly review:

```markdown
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
📊 WEEKLY REVIEW
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

Week of [Start Date] - [End Date]
Generated: [Current Date]

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

## 📈 WEEK AT A GLANCE

[Stats summary]

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

## 🏆 THIS WEEK'S WINS

[Celebration of achievements]

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

## ✅ WHAT WORKED

[Success patterns]

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

## ⚠️ WHAT STRUGGLED

[Challenges + course corrections]

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

## 💡 KEY INSIGHT

[Most important pattern or learning]

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

## 🎯 NEXT WEEK'S FOCUS

[Data-driven priorities for next week]

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

Have a great week! 🚀
```

### Step 4: Optionally Save Report

Ask user:

```
Save this weekly review? (yes/no)

> If yes: Save to reports/weekly-{date}.md
> If no: Just display, don't save
```

---

## Error Handling

**If no check-in data for this week:**
```
⚠️  No check-in data found for this week.

You haven't checked in yet this week. Run /daily-checkin first,
then come back for your weekly review.
```

**If only 1-2 check-ins:**
```
You only checked in [X] times this week.

I can still generate a review, but insights will be limited
with sparse data. Continue? (yes/no)
```

**If databases don't exist:**
```
❌ No habit tracking data found.

Run /onboard to set up your Life OS, then use /daily-checkin
to start tracking.
```

---

## Integration Notes

- Reads from:
  - `databases/habit-tracker.db` (week's data)
  - `user-profile.json` (goals and context)
- Calls agent: `weekly-analyst`
- Optionally saves to: `reports/weekly-{YYYY-MM-DD}.md`

---

## User Experience

**Best time to run:** Sunday evening

**Frequency:** Weekly

**Value:** Course-correct before small issues become big problems

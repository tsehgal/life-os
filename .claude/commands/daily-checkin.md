# Daily Check-in Command

**Command:** `/daily-checkin`

**Purpose:** Evening habit tracking with personalized insights

**Duration:** ~3-4 minutes

---

## Overview

The daily check-in is your accountability system. It:
- Asks about YOUR habits (from user-profile.json)
- Saves responses to database
- Calculates streaks and trends
- Generates personalized insights based on YOUR patterns
- Takes 3-4 minutes total

---

## Process

### Step 1: Check for Today's Entry

```python
import sqlite3
from datetime import datetime

today_str = datetime.now().strftime('%Y-%m-%d')

conn = sqlite3.connect('databases/habit-tracker.db')
cursor = conn.cursor()

cursor.execute('SELECT * FROM daily_habits WHERE date = ?', (today_str,))
existing = cursor.fetchone()

if existing:
    print("⚠️  You already checked in today!")
    print("Check in again? This will overwrite today's data. (yes/no)")
    # If no, exit
```

### Step 2: Launch Check-in Coach Agent

Call the `checkin-coach` agent:

```
Task(
    subagent_type="checkin-coach",
    description="Run daily check-in",
    prompt="""
    Guide user through daily habit check-in.

    Process:
    1. Load user's habits from user-profile.json
    2. Greet user warmly (time-appropriate greeting)
    3. Ask each habit question conversationally
    4. Save responses to databases/habit-tracker.db
    5. Calculate streaks and weekly stats
    6. Analyze patterns in their data
    7. Generate personalized insight
    8. Show stats and insight

    Keep total time under 5 minutes.
    Be encouraging but honest.
    Base insights on actual data patterns.
    """
)
```

Agent handles:
- Asking questions
- Saving to database
- Calculating metrics
- Pattern analysis
- Generating insight

### Step 3: Agent Returns Summary

Agent shows:
- Streak (current consecutive days)
- Weekly stats (habits completed this week)
- Personalized insight based on patterns
- Tomorrow's priority (from user's input)

Example output from agent:
```markdown
✅ Check-in complete for Friday, January 30, 2026

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

📊 YOUR STATS

🔥 Current streak: 12 days
📈 This week: 5/7 days checked in
⚡ Avg energy: 7.4/10
😊 Avg mood: 7.8/10

[Goal 1]: ████████░░ 80% to target
[Goal 2]: ███████░░░ 67% weekly target

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

💡 YOUR INSIGHT

[Personalized insight based on user's actual patterns]

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

Tomorrow's #1 priority: [Their stated priority]

See you tomorrow! 🌟
```

---

## Error Handling

**If user-profile.json doesn't exist:**
```
❌ No user profile found.

Run /onboard first to set up your Life OS.
```

**If database doesn't exist:**
```
Agent creates it automatically on first check-in.
```

**If user skips questions:**
```
Agent allows skipping and continues with remaining questions.
```

---

## Integration Notes

- Reads from: `user-profile.json`
- Calls agent: `checkin-coach`
- Saves to: `databases/habit-tracker.db`
- Calculates: Streaks, weekly stats, patterns

---

## User Experience

**Best time to run:** Evening before bed

**Frequency:** Daily

**Value:** Builds habits through accountability + sees patterns you'd otherwise miss

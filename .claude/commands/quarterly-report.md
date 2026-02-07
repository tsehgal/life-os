# Quarterly Report Command

**Command:** `/quarterly-report`

**Purpose:** Deep 12-week (quarterly) reflection

**Duration:** ~15-20 minutes to read

---

## Overview

The quarterly report is your big-picture review. It:
- Analyzes 12 weeks (one quarter) of data
- Shows macro trends and evolution
- Identifies what worked and what didn't across the entire quarter
- Extracts key lessons learned
- Celebrates real progress
- Suggests next quarter's priorities

---

## Process

### Step 1: Calculate Quarter Dates

```python
from datetime import datetime, timedelta

today = datetime.now()
quarter_start = today - timedelta(days=84)  # ~12 weeks ago
quarter_end = today

# Determine quarter name
quarter_num = (today.month - 1) // 3 + 1
quarter_name = f"Q{quarter_num} {today.year}"
```

### Step 2: Verify Sufficient Data

```python
import sqlite3

conn = sqlite3.connect('databases/habit-tracker.db')
cursor = conn.cursor()

cursor.execute('''
SELECT COUNT(*)
FROM daily_habits
WHERE date BETWEEN ? AND ?
''', (quarter_start.strftime('%Y-%m-%d'), today.strftime('%Y-%m-%d')))

days_with_data = cursor.fetchone()[0]

if days_with_data < 21:  # Less than 3 weeks
    print(f"⚠️  Only {days_with_data} days of data in this quarter.")
    print("Quarterly reports work best with 6+ weeks of data.")
    print("Continue anyway? (yes/no)")
```

### Step 3: Launch Quarterly Analyst Agent

Call the `quarterly-analyst` agent:

```
Task(
    subagent_type="quarterly-analyst",
    description="Generate quarterly report",
    prompt="""
    Conduct deep 12-week (quarterly) review.

    Process:
    1. Load quarter's data from databases/habit-tracker.db
    2. Load user context from user-profile.json
    3. Calculate quarter stats and trends
    4. Identify macro trends (energy, consistency, momentum)
    5. Find strongest patterns and correlations
    6. Extract key breakthroughs (inflection points)
    7. Compare to previous quarter (if exists)
    8. Generate comprehensive report with:
       - Executive summary
       - Quarter's story (narrative arc)
       - Wins and milestones
       - Lessons learned (what worked, what didn't)
       - Key insights and breakthroughs
       - Progress visualization
       - Next quarter priorities (data-driven)

    Keep under 10 minutes reading time.
    Be reflective and big-picture.
    Use user's exact words from check-ins for quotes.
    Base priorities on actual data patterns.
    """
)
```

### Step 4: Agent Generates Report

Agent creates comprehensive quarterly analysis:

```markdown
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
📊 QUARTERLY REPORT
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

Quarter: Q1 2026
Period: January 1 - March 31, 2026
Generated: [Current Date]

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

## 📊 QUARTER IN NUMBERS

[12-week stats summary]

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

## 📖 THE QUARTER'S STORY

[Narrative arc: Month 1, Month 2, Month 3, Overall trajectory]

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

## 🏆 QUARTER'S WINS

[Major milestones, habits formed, personal bests]

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

## 💡 LESSONS LEARNED

[What worked, what didn't work, with evidence]

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

## 🔍 KEY INSIGHTS

[Breakthroughs, hidden patterns, realizations]

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

## 📈 PROGRESS VISUALIZATION

[Visual summary of month-to-month progress]

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

## 🎯 NEXT QUARTER PRIORITIES

[Data-driven focus areas for Q2]

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

This was a quarter of [growth/learning/building].

You're not where you were 12 weeks ago. Keep going. 🚀

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

### Step 5: Save Report

```python
from pathlib import Path

report_path = Path(f"reports/quarterly-{quarter_name}.md")

# Create reports directory if needed
report_path.parent.mkdir(exist_ok=True)

# Agent has already saved the report
print(f"✅ Quarterly report saved: {report_path}")
```

### Step 6: Summary

```markdown
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

📄 Quarterly Report Complete

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

Quarter: {quarter_name}
Period: {start_date} - {end_date}
Days tracked: {days_with_data}/84

Report saved to: reports/quarterly-{quarter_name}.md

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

Top 3 lessons:
1. [Lesson 1]
2. [Lesson 2]
3. [Lesson 3]

Next quarter focus:
→ [Primary priority from data]

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

Time to start Q2 strong. 🚀
```

---

## Error Handling

**If insufficient data:**
```
⚠️  Only {X} days of data in this quarter.

Quarterly reports work best with at least 42 days (6 weeks)
of data. Your report will have limited insights.

Continue anyway? (yes/no)
```

**If no data at all:**
```
❌ No tracking data found for this quarter.

Run /daily-checkin to start building your data, then come
back for your quarterly review in a few weeks.
```

**If databases don't exist:**
```
❌ No habit tracking data found.

Run /onboard to set up Life OS.
```

---

## Integration Notes

- Reads from:
  - `databases/habit-tracker.db` (12 weeks of data)
  - `user-profile.json` (goals and context)
  - Previous quarterly reports (for comparison)
- Calls agent: `quarterly-analyst`
- Saves to: `reports/quarterly-{Q}-{YEAR}.md`

---

## User Experience

**Best time to run:** End of each quarter
- Q1: End of March
- Q2: End of June
- Q3: End of September
- Q4: End of December

**Frequency:** Quarterly (every 12 weeks)

**Value:** Big-picture perspective you can't get from daily/weekly views

# Daily Brief Command

**Command:** `/daily-brief`

**Purpose:** Generate personalized morning intelligence briefing

**Duration:** Generates in ~2-3 min, reading time ~5-10 min

---

## Overview

The daily brief is your personalized morning intelligence system. It:
- Researches topics relevant to YOUR goals (from user-profile.json)
- Filters out stale news and duplicates from yesterday
- Prioritizes based on YOUR stated goals
- Surfaces actionable opportunities (jobs, trends, insights)
- Saves to `/daily-briefs/brief-YYYY-MM-DD.md`

---

## Process

### Step 1: Verify Current Date

```bash
date +"%A, %B %d, %Y"
```

Extract day of week and full date for consistent use throughout brief.

### Step 2: Check for Today's Brief

```python
from pathlib import Path
from datetime import datetime

today_str = datetime.now().strftime('%Y-%m-%d')
brief_path = Path(f'daily-briefs/brief-{today_str}.md')

if brief_path.exists():
    print(f"⚠️  Brief already generated for today: {brief_path}")
    print("Run anyway? (yes/no)")
    # If no, exit
```

### Step 3: Launch Brief Researcher Agent

Call the `brief-researcher` agent using the Task tool:

```
Task(
    subagent_type="brief-researcher",
    description="Research daily intelligence",
    prompt="""
    Research news, trends, and opportunities for today's daily brief.

    Read user-profile.json for topics of interest.
    Check yesterday's brief to avoid duplicates.
    Verify publication dates (only include recent content).
    Prioritize based on user's stated goals.

    Return structured JSON with findings categorized by priority.
    """
)
```

Agent returns JSON with:
- High priority findings (jobs, major news, opportunities)
- Medium priority findings (industry news, trends)
- Low priority findings (context, background)

### Step 4: Launch Brief Writer Agent

Pass research JSON to `brief-writer` agent:

```
Task(
    subagent_type="brief-writer",
    description="Write daily brief",
    prompt="""
    Transform research data into personalized morning brief.

    Input: JSON from brief-researcher agent
    User profile: user-profile.json
    Current date: {day}, {date}

    Write in clear, efficient style.
    Connect each item to user's goals.
    Include action items.
    Keep total reading time under 10 minutes.

    Save to: daily-briefs/brief-{today_str}.md
    """
)
```

Agent creates formatted markdown brief and saves it.

### Step 5: Confirm Completion

```markdown
✅ Daily brief generated!

📄 Saved to: daily-briefs/brief-{YYYY-MM-DD}.md

📊 Brief stats:
• [X] high-priority items
• [X] topics covered
• ⏱️ [X] minutes reading time

🔴 Top actions:
1. [Most urgent action]
2. [Second priority action]

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

Ready to read! ☕
```

---

## Error Handling

**If user-profile.json doesn't exist:**
```
❌ No user profile found.

Run /onboard first to set up your Life OS.
```

**If no high-priority findings:**
```
📰 Quiet news day - no urgent items.

Brief still includes industry context and trends.
Saved to: daily-briefs/brief-{date}.md
```

**If web search fails:**
```
⚠️  Web search unavailable.

Cannot generate brief without internet access.
Try again later.
```

---

## Integration Notes

- Reads from: `user-profile.json`
- Calls agents: `brief-researcher`, `brief-writer`
- Saves to: `daily-briefs/brief-YYYY-MM-DD.md`
- No databases needed
- Requires web search capability

---

## User Experience

**Best time to run:** Morning (5-10 min reading time with coffee)

**Frequency:** Daily

**Value:** Personalized intelligence that surfaces opportunities you'd otherwise miss

# Brain Dump Process Command

**Command:** `/bd-process "Your thought here"`

**Purpose:** Capture and analyze thoughts

**Duration:** ~1-2 minutes per entry

---

## Overview

Brain dump processing helps you:
- Capture thoughts before you forget them
- Analyze patterns across all your dumps
- Extract themes, connections, and insights
- Generate action items from your thinking
- Build a searchable second brain

---

## Process

### Step 1: Capture Raw Thought

```
Usage: /bd-process "Your thought here"

Example:
/bd-process "Feeling overwhelmed by job search + side project.
Not sure which to prioritize this week."
```

### Step 2: Save Raw Entry

```python
from datetime import datetime
from pathlib import Path

# Generate filename
timestamp = datetime.now()
date_str = timestamp.strftime('%Y-%m-%d')
filename = f"{date_str}-dump.txt"

# Save to raw directory
raw_path = Path('braindumps/raw') / filename

# Create directory if needed
raw_path.parent.mkdir(parents=True, exist_ok=True)

# Save entry
with open(raw_path, 'w') as f:
    f.write(user_input)

print(f"✅ Brain dump saved: {raw_path}")
```

### Step 3: Save to Database

```python
import sqlite3

conn = sqlite3.connect('databases/brain-dumps.db')
cursor = conn.cursor()

cursor.execute('''
INSERT INTO entries (date, content)
VALUES (?, ?)
''', (date_str, user_input))

conn.commit()
conn.close()
```

### Step 4: Quick Analysis (Optional)

For immediate analysis, ask user:

```
Want me to analyze patterns across all your brain dumps now?
This reads all your dumps and generates insights. (yes/no)

> If yes, proceed to Step 5
> If no, done (just saved the dump)
```

### Step 5: Launch Analysis Pipeline (If Requested)

**Stage 1: Extract Insights**

Call `insight-extractor` agent:

```
Task(
    subagent_type="insight-extractor",
    description="Extract brain dump insights",
    prompt="""
    Analyze all brain dump files in braindumps/raw/.

    Extract:
    - Recurring themes
    - Evolution over time
    - Hidden connections
    - Key questions
    - Breakthrough moments
    - Top realizations
    - Emotional patterns

    Return structured JSON.
    """
)
```

**Stage 2: Create Beautiful Report**

Pass JSON to `brain-dump-analyst` agent:

```
Task(
    subagent_type="brain-dump-analyst",
    description="Generate brain dump analysis",
    prompt="""
    Transform insights into beautiful visual report.

    Input: JSON from insight-extractor
    Current date: {date}

    Create:
    - ASCII mind map showing themes
    - Top 10 realizations (exact user quotes)
    - Evolution timeline
    - Action items prioritized
    - Breakthrough moments
    - Growth celebration

    Save to: braindumps/analysis/{date}-analysis.md
    """
)
```

### Step 6: Confirm Completion

```markdown
✅ Brain dump processed!

📄 Raw entry saved: braindumps/raw/{date}-dump.txt
💾 Database updated

[If analysis run:]
📊 Analysis report: braindumps/analysis/{date}-analysis.md

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

Top 3 themes:
1. [Theme 1]
2. [Theme 2]
3. [Theme 3]

Biggest insight:
"[Key pattern or realization]"

High-priority actions:
□ [Action 1]
□ [Action 2]

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

---

## Usage Patterns

**Quick capture (no analysis):**
```
/bd-process "Quick thought I want to save"
→ Saved in 10 seconds
→ No analysis, just stored
```

**Capture + analyze:**
```
/bd-process "Longer reflection about career direction..."
→ Saved
→ Run analysis? yes
→ Full analysis report generated
```

**Monthly analysis:**
```
Run /bd-process with analysis once a month to see patterns
across all your dumps.
```

---

## Error Handling

**If no brain dumps exist:**
```
This is your first brain dump!

Saved to: braindumps/raw/{date}-dump.txt

Run /bd-process a few more times, then request analysis to
see patterns.
```

**If only 1-2 dumps:**
```
✅ Saved!

Note: You only have 2 brain dumps. Patterns will be weak with
limited data. Try analyzing after 7-10 dumps for better insights.
```

---

## Integration Notes

- Reads from: `braindumps/raw/*.txt`
- Calls agents: `insight-extractor`, `brain-dump-analyst`
- Saves to:
  - `braindumps/raw/{date}-dump.txt` (raw text)
  - `databases/brain-dumps.db` (searchable)
  - `braindumps/analysis/{date}-analysis.md` (reports)

---

## User Experience

**Best time to run:** Anytime you have a thought to capture

**Frequency:** As needed (some users daily, some weekly)

**Value:** Never lose a thought + see patterns you'd otherwise miss

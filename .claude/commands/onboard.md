# Onboard Command

**Command:** `/onboard`

**Purpose:** Set up Life OS for a new user by collecting their goals, interests, and habits to track.

**Duration:** ~5-10 minutes

---

## Overview

This command walks new users through a guided setup process. It collects:
- Personal info (name, timezone)
- Career goals and interests
- Health/fitness goals (if any)
- Learning goals
- Content projects (if any)
- Daily brief topics
- Habits to track

Then generates a personalized `user-profile.json` and creates initial database structure.

---

## Process

### Step 1: Welcome

```markdown
Welcome to Life OS! 🚀

I'm going to help you set up your personal operating system.

This takes about 5-10 minutes. I'll ask about different areas of
your life, and you can skip anything that doesn't apply.

Ready? Let's go!
```

### Step 2: Basic Info

Use simple questions (not AskUserQuestion, just conversational):

```
What's your name?
> [User enters name]

What timezone are you in? (e.g., America/New_York, Europe/London)
> [User enters timezone]

What time do you usually wake up? (e.g., 06:00)
> [User enters wake time]

Great, [Name]! Now let's talk about your goals...
```

### Step 3: Life Areas (Using AskUserQuestion)

**Question 1: Career**
```json
{
  "questions": [
    {
      "question": "What are you working toward in your career?",
      "header": "Career",
      "multiSelect": false,
      "options": [
        {
          "label": "Job searching / career transition",
          "description": "Looking for new role, changing careers, or leveling up"
        },
        {
          "label": "Growing in current role",
          "description": "Building skills, seeking promotion, expanding impact"
        },
        {
          "label": "Building a business/side project",
          "description": "Entrepreneurship, freelancing, or side hustle"
        },
        {
          "label": "Not focused on career right now",
          "description": "Skip career tracking"
        }
      ]
    }
  ]
}
```

**If career-focused, follow up:**
```
What's your current role?
> [User enters]

What's your target role or goal?
> [User enters]

Which industries interest you? (comma-separated)
> [User enters]

Any specific companies you're targeting? (comma-separated, optional)
> [User enters]
```

**Question 2: Health & Fitness**
```json
{
  "questions": [
    {
      "question": "Do you have health or fitness goals you want to track?",
      "header": "Health",
      "multiSelect": false,
      "options": [
        {
          "label": "Yes - specific fitness goal",
          "description": "Building strength, losing weight, running, etc."
        },
        {
          "label": "Yes - general wellness",
          "description": "Sleep, energy, stress management"
        },
        {
          "label": "No - skip fitness tracking",
          "description": "Not tracking health/fitness"
        }
      ]
    }
  ]
}
```

**If yes, follow up:**
```
What's your specific goal?
> [User enters, e.g., "Run a marathon by December"]
```

**Question 3: Learning**
```
What skills are you learning or want to learn? (comma-separated, or "none")
> [User enters, e.g., "Python, Data analysis, Public speaking"]
```

**Question 4: Content/Projects**
```json
{
  "questions": [
    {
      "question": "Are you creating content or working on projects?",
      "header": "Projects",
      "multiSelect": false,
      "options": [
        {
          "label": "Yes - newsletter/blog",
          "description": "Writing content regularly"
        },
        {
          "label": "Yes - YouTube/podcast",
          "description": "Creating video or audio content"
        },
        {
          "label": "Yes - side project/startup",
          "description": "Building a product or service"
        },
        {
          "label": "No content projects",
          "description": "Skip content tracking"
        }
      ]
    }
  ]
}
```

**If yes, follow up:**
```
What's your project called?
> [User enters]

What's it about? (topic/focus)
> [User enters]

What's your goal for it?
> [User enters, e.g., "Reach 1000 subscribers"]
```

### Step 4: Daily Brief Topics

```
What topics should I research for your daily morning brief?

These are the subjects I'll search for news, trends, and opportunities
on every morning.

Examples:
- AI trends
- FinTech news
- Product design jobs
- Your industry
- Skills you're learning

Enter your topics (comma-separated):
> [User enters topics]
```

### Step 5: Generate Habit List (AI-Suggested)

Based on their goals, suggest habits to track:

```python
import json

# Analyze user's goals
career_focused = user_data['career']['target_role'] != ""
fitness_focused = user_data['health']['tracking_fitness']
content_focused = user_data['content']['has_project']
learning_focused = len(user_data['learning_goals']) > 0

# Generate suggested habits
suggested_habits = []

# Universal habits
suggested_habits.extend([
    {"id": "energy", "question": "Energy level today? (1-10)", "type": "scale"},
    {"id": "mood", "question": "Mood today? (1-10)", "type": "scale"},
    {"id": "one_win", "question": "One win from today?", "type": "text"},
    {"id": "tomorrow_priority", "question": "Tomorrow's #1 priority?", "type": "text"}
])

# Career-related habits
if career_focused:
    if "job search" in user_data['career']['target_role'].lower():
        suggested_habits.extend([
            {"id": "job_applications", "question": "How many job applications?", "type": "number"},
            {"id": "networking", "question": "How many networking calls/chats?", "type": "number"}
        ])
    else:
        suggested_habits.append(
            {"id": "career_work", "question": "Did you work on career goals today?", "type": "boolean"}
        )

# Fitness habits
if fitness_focused:
    suggested_habits.extend([
        {"id": "workout_completed", "question": "Did you complete your workout?", "type": "boolean"},
        {"id": "steps", "question": "How many steps today?", "type": "number"}
    ])

# Content habits
if content_focused:
    suggested_habits.extend([
        {"id": "content_work", "question": "Did you work on your content project?", "type": "boolean"},
        {"id": "content_published", "question": "Did you publish content today?", "type": "boolean"}
    ])

# Learning habits
if learning_focused:
    suggested_habits.append(
        {"id": "learning_minutes", "question": "How many minutes of learning/practice?", "type": "number"}
    )
```

**Present to user:**
```markdown
Based on your goals, here's what I recommend tracking daily:

UNIVERSAL (Everyone):
□ Energy level (1-10)
□ Mood (1-10)
□ One win from today
□ Tomorrow's #1 priority

[CAREER SECTION if applicable]
□ Job applications submitted
□ Networking calls/chats

[FITNESS SECTION if applicable]
□ Workout completed
□ Daily steps

[CONTENT SECTION if applicable]
□ Work on project
□ Content published

[LEARNING SECTION if applicable]
□ Learning/practice minutes

Does this look right? (You can customize after setup)
> yes/no

[If no: "What would you like to add or remove?"]
```

### Step 6: Generate user-profile.json

```python
import json
import os
from datetime import datetime

profile = {
    "user": {
        "name": name,
        "timezone": timezone,
        "wake_time": wake_time
    },
    "career": {
        "current_role": current_role,
        "target_role": target_role,
        "industries_of_interest": industries.split(',') if industries else [],
        "target_companies": companies.split(',') if companies else []
    },
    "content": {
        "has_project": content_project != "",
        "project_name": project_name,
        "project_topic": project_topic,
        "project_goal": project_goal
    },
    "health": {
        "tracking_fitness": fitness_goal != "",
        "fitness_goal": fitness_goal
    },
    "learning_goals": learning_goals.split(',') if learning_goals else [],
    "daily_brief_topics": brief_topics.split(','),
    "daily_habits": suggested_habits,
    "goals": {
        "career": target_role,
        "personal": project_goal if content_project else "",
        "health": fitness_goal
    }
}

# Save profile
with open('user-profile.json', 'w') as f:
    json.dump(profile, indent=2, fp=f)

print("✅ user-profile.json created!")
```

### Step 7: Create Database Structure

```python
import sqlite3

# Create habit tracker database
conn = sqlite3.connect('databases/habit-tracker.db')
cursor = conn.cursor()

# Build dynamic table based on user's habits
columns = ["date TEXT PRIMARY KEY"]

for habit in suggested_habits:
    if habit['type'] == 'boolean':
        columns.append(f"{habit['id']} INTEGER")
    elif habit['type'] == 'number':
        columns.append(f"{habit['id']} INTEGER")
    elif habit['type'] == 'scale':
        columns.append(f"{habit['id']} INTEGER")
    elif habit['type'] == 'text':
        columns.append(f"{habit['id']} TEXT")

columns.append("created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP")

create_table_sql = f'''
CREATE TABLE IF NOT EXISTS daily_habits (
    {', '.join(columns)}
)
'''

cursor.execute(create_table_sql)
conn.commit()
conn.close()

print("✅ Database created!")

# Create brain dumps database
conn2 = sqlite3.connect('databases/brain-dumps.db')
cursor2 = conn2.cursor()

cursor2.execute('''
CREATE TABLE IF NOT EXISTS entries (
    id INTEGER PRIMARY KEY AUTOINCREMENT,
    date TEXT,
    content TEXT,
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
)
''')

conn2.commit()
conn2.close()

print("✅ Brain dumps database created!")
```

### Step 8: Completion Message

```markdown
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

🎉 Your Life OS is ready!

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

I've created:
✅ user-profile.json (your personalized config)
✅ databases/habit-tracker.db (for daily check-ins)
✅ databases/brain-dumps.db (for thought processing)

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

## Try These Commands:

**Morning routine:**
/daily-brief
→ Personalized intelligence on your topics

**Evening routine:**
/daily-checkin
→ Track today's habits, build streaks

**Anytime:**
/bd-process "Your thought here"
→ Process and analyze brain dumps

**Weekly:**
/weekly-checkin
→ Review the week, plan ahead

**Quarterly:**
/quarterly-report
→ Deep 12-week reflection

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

## Recommended Workflow:

**Morning (5-10 min):**
1. Run /daily-brief
2. Read with coffee
3. Note action items

**Evening (3-4 min):**
1. Run /daily-checkin
2. Answer habit questions
3. See your streak/progress

**Sunday evening:**
1. Run /weekly-checkin
2. Reflect and plan

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

Start with your first daily brief:

/daily-brief

Let's go! 🚀
```

---

## Notes

- Use `AskUserQuestion` tool for multi-option questions (career, health, content)
- Use simple conversational prompts for text inputs (name, goals, topics)
- AI suggests habits based on goals (not blank slate)
- User can customize habits list before finalizing
- Creates both databases upfront
- Provides clear next steps

This is the entry point. Make it welcoming and efficient.

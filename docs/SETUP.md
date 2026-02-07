# Life OS - Quick Start Guide

Get up and running in 5 minutes.

---

## Prerequisites

**Required:**
- [Claude Code CLI](https://claude.com/code) installed
- Terminal/command line access

**Note:** Life OS is terminal-based. No GUI required.

---

## Installation

### Step 1: Clone the Repository

```bash
git clone https://github.com/yourusername/life-os-shareable.git
cd life-os-shareable
```

### Step 2: Open in Claude Code

```bash
claude .
```

This opens the project in Claude Code CLI.

---

## Onboarding (5 Minutes)

### Run the Onboarding Command

```
/onboard
```

The onboarding wizard will:
1. Ask about your life areas (career, health, content, learning, etc.)
2. Collect your goals and interests
3. Ask what topics you want in your daily brief
4. Suggest daily habits based on your goals
5. Generate your personalized `user-profile.json`
6. Create your databases

**Total time:** 5 minutes

---

## Your Daily Routine

### Morning: Read Your Brief (5-10 min)

```
/daily-brief
```

Get personalized intelligence on:
- Topics you care about
- Industries you're tracking
- Companies you're targeting
- Skills you're learning

**Output:** Saved to `daily-briefs/brief-YYYY-MM-DD.md`

### Evening: Check In (3-4 min)

```
/daily-checkin
```

Track your habits, get insights:
- Answer questions about YOUR habits (from your profile)
- See your current streak
- Get personalized insights based on patterns
- Set tomorrow's #1 priority

**Output:** Saved to `databases/habit-tracker.db`

---

## Additional Commands

### Capture Thoughts Anytime

```
/bd-process "Your thought here"
```

Example:
```
/bd-process "Feeling overwhelmed by job search and side project.
Not sure which to prioritize this week."
```

The system will:
- Save your thought
- Optionally analyze patterns across all your brain dumps
- Generate insights and action items

### Weekly Review (Sundays)

```
/weekly-checkin
```

Review the week:
- Wins and achievements
- What worked, what struggled
- Patterns in your data
- Next week's focus

### Quarterly Deep Dive (End of Quarter)

```
/quarterly-report
```

12-week reflection:
- Macro trends and evolution
- Lessons learned
- Key breakthroughs
- Next quarter priorities

**Best times to run:**
- Q1: End of March
- Q2: End of June
- Q3: End of September
- Q4: End of December

---

## File Structure

After onboarding, your directory looks like this:

```
life-os-shareable/
├── user-profile.json          # YOUR personalized config
├── user-background.md         # YOUR story (optional)
├── databases/
│   ├── habit-tracker.db       # Daily check-in data
│   └── brain-dumps.db         # Brain dump entries
├── daily-briefs/
│   └── brief-YYYY-MM-DD.md    # Daily brief reports
├── braindumps/
│   ├── raw/                   # Raw brain dump text
│   └── analysis/              # Analysis reports
└── reports/
    ├── weekly-YYYY-MM-DD.md   # Weekly reviews
    └── quarterly-Q#-YEAR.md   # Quarterly reports
```

**Privacy:** Your data stays local. Nothing uploaded to cloud.

---

## Tips for Success

### 1. Build the Streak
- Run `/daily-checkin` every evening
- Consistency > perfection
- Streaks create accountability

### 2. Use the Brief Actively
- Read it with your morning coffee
- Take action on insights
- Adjust topics if not valuable

### 3. Brain Dump Liberally
- Capture thoughts before you forget
- Run analysis monthly to see patterns
- Use insights for course correction

### 4. Review Regularly
- Weekly check-ins = small adjustments
- Quarterly reports = big strategic shifts
- Don't skip these - they compound

---

## Customizing Your Setup

### Change Your Habits

Edit `user-profile.json`:

```json
{
  "daily_habits": [
    {
      "id": "workout",
      "question": "Did you work out today?",
      "type": "boolean"
    },
    {
      "id": "writing",
      "question": "How many minutes of writing?",
      "type": "number"
    }
  ]
}
```

Types: `boolean`, `number`, `text`, `rating` (1-10)

### Change Brief Topics

Edit `user-profile.json`:

```json
{
  "daily_brief_topics": [
    "AI product management",
    "UX design trends",
    "Your Custom Topic"
  ]
}
```

### Add Goals

Edit `user-profile.json`:

```json
{
  "goals": {
    "career": "Senior PM at top tech company",
    "health": "Run a marathon",
    "learning": "Ship an AI product"
  }
}
```

---

## Troubleshooting

### "No user profile found"

**Problem:** You haven't run onboarding yet.

**Solution:** Run `/onboard` to set up your profile.

### "Already checked in today"

**Problem:** You already completed today's check-in.

**Solution:** The command will ask if you want to overwrite. Say yes if you want to update today's data.

### "No data found"

**Problem:** You haven't used the system enough yet.

**Solution:** Keep using `/daily-checkin` for a few days, then insights will become more valuable.

---

## Next Steps

1. Run `/onboard` (if you haven't already)
2. Try `/daily-brief` to see your first personalized brief
3. Run `/daily-checkin` tonight to start your streak
4. Capture a thought with `/bd-process "your thought"`
5. Check back Sunday for `/weekly-checkin`

---

## Need Help?

- **Architecture:** Read `docs/ARCHITECTURE.md` to understand how the system works
- **Product Decisions:** Read `docs/DECISIONS.md` to see why features were included/excluded
- **Full Context:** Read `PRODUCT-CONTEXT.md` for complete system documentation

---

**You're all set. Start your first check-in tonight and build the streak.**

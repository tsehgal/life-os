# Life OS

**Your AI-Powered Personal Operating System**

> Built by [Tarun Sehgal](https://linkedin.com/in/tarunsehgal) — because productivity tools should be personal.

Inspired by [Peter Yang and Alex Finn's conversation](https://www.youtube.com/watch?v=D0nDWQdN3F4) on building a "life operating system" — I turned the idea into a working tool.

---

## The Problem

As a Senior PM juggling work, side projects, fitness goals, and learning AI - I was drowning.

**Information overload:** 47 newsletters, 12 Slack communities, endless LinkedIn posts. What's actually relevant to my goals?

**Broken habits:** Streaks broken, goals forgotten, no accountability.

**Scattered thoughts:** Great ideas in random notes. No way to see patterns or connections.

**Existing tools didn't work:**
- Notion: Love it but too flexible, lacked feedback loop
- Todoist: Checked boxes, didn't understand why I kept failing
- Newsletters: Generic content, not personalized to my goals
- Habit trackers: Data collection without insights

**What I needed:** AI as a coach, not just an assistant.

Something that:
- Filters information to what matters for MY goals
- Holds me accountable with pattern-based insights
- Processes my thoughts and shows me what I'm not seeing
- Prompts reflection before small issues become big problems

That's Life OS (yeah, the name's a bit much).

---

## The Solution

Life OS is a **configurable AI-powered system** that runs in your terminal via Claude Code CLI.

### 5 Core Commands

#### 1. `/daily-brief` - Your Personalized Morning Intelligence

Not another generic news aggregator.

**What it does:**
- Researches topics YOU care about (from your profile)
- Verifies publication dates (no old news)
- Connects findings to YOUR goals
- 5-10 minute read with your coffee

**Example topics:**
- "Remote engineering jobs in fintech"
- "Sustainable investing trends"
- "Strength training research for hypertrophy"

**Output:** Saved to `daily-briefs/brief-YYYY-MM-DD.md`

---

#### 2. `/daily-checkin` - Your Evening Accountability Partner

Not a habit tracker. A coach.

**What it does:**
- Asks about YOUR habits (configured during onboarding)
- Saves to local database
- Calculates streaks and weekly stats
- Analyzes patterns you'd miss ("You're 23% more productive on workout days")
- Shows personalized insights based on YOUR data

**Example questions (yours will be different):**
- "Did you apply to jobs today?"
- "How many minutes of AI learning?"
- "Energy level (1-10)?"
- "One win from today?"

**Takes:** 3-4 minutes
**Builds:** Accountability through consistency

---

#### 3. `/bd-process` - Your Thought Processor

Capture thoughts before you forget. See patterns you'd otherwise miss.

**What it does:**
- Quick capture: `/bd-process "your thought here"`
- Saves to local database
- Optionally analyzes ALL your brain dumps to find:
  - Recurring themes
  - Hidden connections
  - Evolution over time
  - Action items you keep mentioning but not doing

**Example:**
```
/bd-process "Feeling overwhelmed by job search + newsletter + fitness.
Not sure which to prioritize this week."
```

**Output:**
- Thought saved in 10 seconds
- Optional analysis report showing patterns across all dumps
- Prioritized action items

---

#### 4. `/weekly-checkin` - Your Sunday Night Review

Weekly course correction before small problems become big ones.

**What it does:**
- Reviews the week's check-in data
- Celebrates wins (streaks, personal bests, goals hit)
- Identifies what worked and what struggled
- Spots patterns (Mondays always low energy? Gym days = better mood?)
- Suggests next week's focus based on data, not motivation

**Takes:** 10-15 minutes
**Prevents:** Burning weeks on the wrong priorities

---

#### 5. `/quarterly-report` - Your 12-Week Deep Dive

Big-picture reflection you can't get from daily/weekly views.

**What it does:**
- Analyzes 12 weeks (one quarter) of data
- Shows macro trends and evolution
- Extracts lessons learned (what worked, what didn't, with evidence)
- Identifies breakthrough moments
- Suggests next quarter priorities

**Takes:** 15-20 minutes to read
**Prevents:** Wasting entire quarters on tactics that don't work

---

## Quick Start (5 Minutes)

### Prerequisites

- [Claude Code CLI](https://claude.com/code) installed
- Terminal access

### Installation

```bash
git clone https://github.com/yourusername/life-os-shareable.git
cd life-os-shareable
claude .
```

### Onboarding

```
/onboard
```

The onboarding wizard asks:
1. What life areas are you focused on? (career, health, content, learning, etc.)
2. What are your current goals?
3. What topics do you want in your daily brief?
4. (AI suggests relevant habits based on your goals)
5. Customize or accept suggested habits

**Total time:** 5 minutes

**Output:**
- `user-profile.json` created (your personalized config)
- Databases initialized
- Ready to use

### Your First Day

**Morning:**
```
/daily-brief
```
Get your first personalized intelligence briefing.

**Evening:**
```
/daily-checkin
```
Start your accountability streak.

**Anytime:**
```
/bd-process "your thought here"
```
Capture a thought before you forget.

**Sunday:**
```
/weekly-checkin
```
Review the week and plan next week.

**End of Quarter:**
```
/quarterly-report
```
Deep reflection on 12 weeks of progress.

---

## Architecture

### Config-Driven System

**Problem:** Original Life OS was hardcoded to my specific goals.

**Solution:** Single source of truth: `user-profile.json`

```json
{
  "user": {
    "name": "Your Name"
  },
  "career": {
    "current_role": "Your Role",
    "target_role": "Your Goal"
  },
  "daily_brief_topics": [
    "YOUR topics here"
  ],
  "daily_habits": [
    {
      "id": "your_habit",
      "question": "YOUR question",
      "type": "boolean"
    }
  ],
  "goals": {
    "career": "YOUR goal",
    "health": "YOUR goal"
  }
}
```

**All commands read from this file.** Change the config, change the experience.

**Result:** Anyone can use it in 5 minutes.

### Agent-Based Architecture

**Commands orchestrate, agents work.**

**7 Specialized Agents:**
1. `insight-extractor` - Extract themes from brain dumps
2. `brain-dump-analyst` - Format analysis beautifully
3. `brief-researcher` - Research web for daily brief
4. `brief-writer` - Write personalized brief
5. `checkin-coach` - Guide daily check-in
6. `weekly-analyst` - Generate weekly review
7. `quarterly-analyst` - Generate quarterly report

**Why agents?**
- Consistent, high-quality output (200-500 line instructions)
- Reusable across commands
- Maintainable (update agent, all commands improve)
- Pipeline architecture (extract → format workflows)

**See full architecture:** [docs/ARCHITECTURE.md](docs/ARCHITECTURE.md)

---

## Local-First & Privacy

**Your data never leaves your machine** (except AI API calls for processing).

**Databases:** SQLite (not cloud)
**Reports:** Markdown files (not web UI)
**Config:** JSON on disk (not remote server)

**No tracking. No analytics. No cloud uploads.**

**You own your data.**

---

## Impact & Validation

### Personal Impact (60+ Days Using Original Version)

**Habits:**
- 60-day check-in streak (longest ever)
- 87% habit completion rate
- Actually hit fitness macros 6/7 days per week

**Information:**
- 30 minutes/day saved (no more endless newsletter scrolling)
- Only read what's relevant to my goals
- Caught relevant job posting 2 days after it went live

**Reflection:**
- Discovered pattern: workout days = 40% higher productivity
- Identified blocker: Sunday planning prevents Monday chaos
- Course-corrected before burning entire month on wrong priority


---

## Tech Stack

**Runtime:** Claude Code CLI
**AI:** Claude (Anthropic API)
**Databases:** SQLite
**Reports:** Markdown
**Config:** JSON
**Interface:** Terminal (CLI)

**Dependencies:**
- Claude Code CLI
- Python 3.x
- SQLite 3

**No external APIs required** (except Claude, included with Claude Code).

---

## Documentation

**Quick Start:** [docs/SETUP.md](docs/SETUP.md) - Get running in 5 minutes

**System Design:** [docs/ARCHITECTURE.md](docs/ARCHITECTURE.md) - How it's built

---

## Contributing

If you want to fork it and build your own version:

1. Fork the repo
2. Run `/onboard` to set up your profile
3. Customize agents/commands to your needs
4. Ship your own version

**License:** MIT - Make it yours.

---

## Contact

**Tarun Sehgal** — [LinkedIn](https://linkedin.com/in/tarunsehgal)
Open to chatting about AI product management.

---

## Key Takeaways

### If You're a PM Learning AI:

Key lessons:
1. **Config-driven > Hardcoded** - Build for anyone, not just yourself
2. **Agent architecture > Inline prompts** - Quality and consistency matter
3. **Ship focused > Ship everything** - 80/20 principle
4. **Local-first > Cloud-first** - Privacy and ownership
5. **CLI first > GUI first** - Validate faster, polish later
6. **Cut ruthlessly** - Show what you didn't build and why

### If You're Looking for a Personal OS:

**Try it:**
1. Clone the repo
2. Run `/onboard` (5 minutes)
3. Use it for 7 days
4. Report back if it's actually useful

Built with AI. Powered by accountability. Driven by data.


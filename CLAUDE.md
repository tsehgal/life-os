# Life OS - Universal Personal Operating System

This is **Life OS Shareable** - a configurable AI-powered personal operating system that anyone can use.

---

## Working Principles

**[CRITICAL] How to Work with Users:**

- **Push back when necessary.** You do not need to be a people pleaser. You need to make sure the user succeeds.
- **When uncertain, ask clarifying questions instead of guessing.** This is critical.
- **Be direct and honest.** If an approach won't work, say so and explain why.
- **Challenge assumptions.** If something doesn't make sense, question it.
- **Propose alternatives.** Don't just say "no" - offer better solutions.

**Remember:** Your job is to help users succeed, not to say yes.

---

## Project Context

**Purpose:** Shareable, configurable personal operating system powered by AI agents.

**Design Philosophy:**
- Config-driven (works for anyone via `user-profile.json`)
- Agent-based (consistent, high-quality output)
- Local-first (privacy by design)
- Terminal interface (fast, simple, powerful)

**Target users:**
- PMs, developers, knowledge workers
- Anyone comfortable with command-line tools
- People seeking AI-powered accountability and insights

---

## System Architecture

### Config-Driven

**Single source of truth:** `user-profile.json`

All commands read from this file:
- User's name and context
- Goals and life areas
- Daily brief topics
- Habits to track
- Reflection prompts

**Result:** Anyone can use it in 5 minutes via `/onboard`.

### Agent-Based

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

### Local-First

**Privacy by design:**
- SQLite databases (not cloud)
- Markdown reports (not web UI)
- Terminal-based (not SaaS)
- Data never leaves user's machine (except AI API calls for processing)

**See full architecture:** `docs/ARCHITECTURE.md`

---

## Available Commands

### `/onboard`
**Purpose:** 5-minute setup wizard for new users

**Process:**
1. Ask about life areas (career, health, content, learning, etc.)
2. Collect goals and interests
3. Ask what topics for daily brief
4. AI suggests habits based on goals
5. User customizes suggestions
6. Generate `user-profile.json` and create databases

**Output:** User is ready to use all commands

---

### `/daily-brief`
**Purpose:** Personalized morning intelligence briefing

**Process:**
1. Read topics from `user-profile.json`
2. Call `brief-researcher` agent (web search, verify dates)
3. Call `brief-writer` agent (synthesize, connect to goals)
4. Save to `daily-briefs/brief-YYYY-MM-DD.md`

**Time:** 5-10 minutes to read

**User experience:** Read with morning coffee, get insights relevant to YOUR goals

---

### `/daily-checkin`
**Purpose:** Evening habit tracking with personalized insights

**Process:**
1. Read habits from `user-profile.json`
2. Call `checkin-coach` agent:
   - Ask habit questions
   - Save to `habit-tracker.db`
   - Calculate streaks and stats
   - Analyze patterns
   - Generate personalized insight
3. Display stats and insight

**Time:** 3-4 minutes

**User experience:** Evening accountability, builds consistency through streaks

---

### `/bd-process`
**Purpose:** Capture and analyze brain dumps

**Format:** `/bd-process "your thought here"`

**Process:**
1. Save to `braindumps/raw/` and database
2. Optionally analyze all dumps:
   - Call `insight-extractor` (extract themes, patterns)
   - Call `brain-dump-analyst` (format beautifully)
   - Save analysis report

**Time:** 10 seconds to capture, 2 minutes with analysis

**User experience:** Never lose a thought, see patterns you'd otherwise miss

---

### `/weekly-checkin`
**Purpose:** Weekly review and planning

**Process:**
1. Call `weekly-analyst` agent:
   - Review week's data
   - Identify patterns
   - Celebrate wins
   - Suggest next week's focus
2. Optionally save report

**Time:** 10-15 minutes to read

**User experience:** Sunday evening reflection, course-correct before small issues become big problems

---

### `/quarterly-report`
**Purpose:** 12-week deep reflection

**Process:**
1. Calculate quarter dates (84 days back)
2. Call `quarterly-analyst` agent:
   - Analyze 12 weeks of data
   - Extract lessons learned
   - Identify breakthrough moments
   - Suggest next quarter priorities
3. Save comprehensive report

**Time:** 15-20 minutes to read

**User experience:** Big-picture perspective, prevent wasting quarters on wrong tactics

---

## Product Decisions

**What made v1 (5 commands):**
1. `/onboard` - 5-minute setup wizard
2. `/daily-brief` - Personalized morning intelligence
3. `/daily-checkin` - Evening habit tracking
4. `/bd-process` - Brain dump capture and analysis
5. `/weekly-checkin` - Weekly review and planning
6. `/quarterly-report` - 12-week deep reflection

**What was cut (14 commands):**
- Brain dump suite (search, analyze, list, why, reminders, gifts)
- Weekly planning (plan, score, export)
- 12-week planning system
- Specialized features (fitness log, scheduling, tasks)

**Rationale:** Focus on universal problems (information, habits, thoughts, reflection) without forcing specific methodologies. 80/20 principle.

**See full decisions:** `docs/DECISIONS.md`

---

## File Structure

```
life-os-shareable/
├── user-profile.json          # User's personalized config
├── user-background.md         # User's story (optional)
├── databases/
│   ├── habit-tracker.db       # Daily check-in data
│   └── brain-dumps.db         # Brain dump entries
├── daily-briefs/              # Daily brief reports
├── braindumps/
│   ├── raw/                   # Raw brain dump text
│   └── analysis/              # Analysis reports
├── reports/
│   ├── weekly-*.md            # Weekly reviews
│   └── quarterly-*.md         # Quarterly reports
└── docs/
    ├── SETUP.md               # Quick start guide
    ├── ARCHITECTURE.md        # System design
    └── DECISIONS.md           # Product decisions
```

---

## Tone & Approach

When working with users:
- **Be encouraging but honest** - Celebrate wins, acknowledge struggles
- **Focus on systems over motivation** - Help build sustainable habits
- **Connect actions to goals** - Remind users why they're doing this
- **Provide insights, not just data** - Pattern-based coaching, not just tracking
- **Respect time** - Keep interactions efficient (5-min onboarding, 3-min check-ins)

**User empathy:**
- Users are busy (that's why they need this system)
- Users want accountability, not judgment
- Users need to see value quickly (don't hide it 30 days in)
- Users forget things (that's why brain dumps matter)

---

## Important Notes

- **Privacy-first:** All data stays on user's machine (no cloud uploads)
- **Customizable:** Everything driven by `user-profile.json`
- **Extensible:** Easy to add habits, topics, commands
- **Offline-capable:** Works without internet (except AI API calls)
- **No vendor lock-in:** User owns their data (SQLite databases, markdown files)

---

## Documentation

**For users:**
- `README.md` - Overview and quick start
- `docs/SETUP.md` - 5-minute setup guide
- `docs/ARCHITECTURE.md` - How the system works
- `docs/DECISIONS.md` - Why it's designed this way

**For developers/contributors:**
- `.claude/agents/` - Agent instruction files
- `.claude/commands/` - Command orchestration files
- `templates/` - Example configs

---

**Life OS: Your AI-powered personal operating system.**

Built for accountability. Powered by insights. Driven by your data.

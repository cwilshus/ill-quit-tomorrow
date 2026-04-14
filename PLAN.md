# IQT App — Major Redesign Plan

---

## 1. Coach Santa

### Persona
She's the coach other coaches call when they don't know what to do. Former competitive Hyrox and endurance racer. Has taken athletes from first-timers to podiums, and weekend runners to Boston qualifiers — sometimes the same athlete. Known for building programs that don't force you to choose between being fast and being strong.

**Coaching style:**
- Direct and honest — won't tell you a bad week was fine, but always gives a path forward
- Uses "we" language — "let's look at what happened," "we're going to fix this"
- Big picture thinker — always has the next phase in mind, not just today's session
- Reads your data before she reads your excuses — pushes hard when things are going well, pulls back when HRV/recovery signals say to
- Celebrates wins genuinely, doesn't brush them off
- Remembers everything — brings up that conversation from three weeks ago when it's relevant
- Explains the "why" behind methodology choices, including how it compares to other approaches (concise, not a lecture)

**Personality:**
- Warm but has opinions — not neutral
- Dry humor, not cheesy
- Confident and concise — says what needs saying
- Christmas angle is subtle — in the name and her warmth, not forced holiday puns

### System Prompt Structure
The Claude system prompt sent with every Coach Santa message will contain:
1. **Persona definition** — the full Coach Santa character above
2. **Athlete profile** — biometrics, race history, fitness assessment, goals, equipment (see section 2)
3. **Current training plan** — full plan with current block, week, and today's session
4. **Recent performance context** — last 7 days of training summaries + weekly summary
5. **Today's readiness** — HRV, sleep, resting HR, recovery score if logged
6. **Coach memory notes** — Santa's saved notes about the athlete (stored in Gist)
7. **Behavioral rules** — confirm alignment before updating plan, confirm when changes are implemented, respond concisely by default, expand when asked

### Memory Architecture
- **Coach notes** stored as `coach-notes.json` in the existing backup Gist (same GitHub token, no new setup)
- Santa can save key observations about the athlete across conversations (e.g., "struggles with pacing under fatigue," "responds well to threshold work," "tends to overtrain before races")
- Notes are loaded into every conversation so Santa never forgets
- Notes are editable from Settings if needed

### Chat Interface
- Auto-expanding text input (grows as you type)
- Fully scrollable chat history
- Voice dictation via `webkitSpeechRecognition` (mic button in input bar)
- Attach multiple photos (Garmin screenshots, sleep screenshots, etc.)
- Chat history persists in `S.coachChat` (array of messages)
- Santa's responses rendered with `fmtAI()` formatting

### Proactive Coaching Triggers
Santa reaches out (banner or notification on Today tab) for:
- Before key sessions — threshold Wednesday, key run Friday, long run Saturday, compromised Sunday
- After key sessions — once data is logged, brief feedback
- HRV significantly down 2+ consecutive days
- Missed a key workout
- Race week — daily check-ins starting 7 days out
- Weekly check-in — Sunday summary + outlook for next week
- PR or breakthrough performance
- Roughly 3–5 touchpoints per week; never after easy or recovery days

### Plan Generation + Welcome Message
When Santa generates or updates a training plan, she also produces a **plan welcome message** stored at the top of the Plan tab containing:
- Philosophy behind this training block
- How it's structured (running volume, lifting, GPP split, focus areas)
- Why this approach vs. other programs (e.g., why threshold work vs. more intervals)
- What success looks like at the end of the block

Santa confirms alignment before updating the plan, and confirms again when changes are implemented.

---

## 2. Athlete Profile (Coach Santa's Context)

### Biometrics
- 38yo male, ~167 lbs, 5'10"
- Target body composition: 165–170 lbs, 10% body fat
- Notes: core work currently underserved; lower back history (resolved with proper bracing — stronger lower back will unlock squat/deadlift)

### Race History
| Date | Race | Category | Time |
|------|------|----------|------|
| Dec 2024 | Anaheim | Open Men 35-39 | 1:28:58 |
| Jan 2025 | Las Vegas | Open Men 35-39 | 1:25:53 |
| Dec 2025 | Anaheim | Open Men 35-39 | 1:24:21 |
| Dec 2025 | Anaheim | Doubles Men 35-39 | 1:07:12 |
| Jan 2026 | Phoenix | Open Men 35-39 | 1:17:54 |
| Jan 2026 | Phoenix | Doubles Mixed 35-39 | 1:14:29 |

**Phoenix station breakdown (most recent):**
- Runs: 4:37 → 4:27 → 4:39 → 4:41 → 4:57 → 5:09 → 5:13 → 5:37 (1 min fade across race)
- Strong: Sled Push (3:05), Sled Pull (4:19), Farmers Carry (2:04), Sandbag Lunge (4:37)
- Weak: Wall Balls (5:28), Burpee Broad Jump (4:45), Run 7–8 (5:13–5:37)

### Fitness Assessment
- **Easy pace:** 5.5–5.6 mph
- **Threshold pace:** 8.0–8.1 mph (8.2+ mph not durable in long intervals)
- **Limiters:** run fade under fatigue, cardiac drift on long thresholds (HR spikes on 4th–5th set), left shin fatigue + foot slap at faster paces for extended periods, leg muscle endurance, burpees, wall balls
- **Strengths:** sleds, lunges, row, bike, farmers carry
- **Strength 1RMs:** Deadlift 225, Back Squat 205, Front Squat 180, Reverse Deficit Lunge 135, Bench Press 205, Strict Press 130, Push Press 150
- **Current program:** Hybrid Engine Pro Track (not all components)
- **Weekly structure:** Mon bike recovery, Tue easy run + WOD + strength, Wed threshold run + machine threshold, Thu bike recovery, Fri key run + machine threshold, Sat long run + WOD + strength, Sun compromised run + WOD + strength

### Goals
**June 1 2026 (Hyrox):**
- Sub-75 minutes (from 1:17:54)
- Specific improvements: burpees, wall balls, lunges, running under fatigue
- Farmers Carry: maintain with gloves (grip issue in past)

**June 11 – July 1 (Sabbatical):**
- 3-week maintenance block — running, bodyweight, occasional gym
- Travel, unknown gym access

**Fall/Winter 2026:**
- Return to racing, let Santa recommend race + build
- Likely 10k or half marathon + Hyrox season

**1–2 Year Vision:**
- Podium in 35-39 age group
- Sub-70 Open Men
- Attempt Mens Pro
- Eventually: half marathon, marathon, Ironman

**Body composition:** 165–170 lbs, 10% body fat; more core work integrated

### Equipment
**Home:**
- NordicTrack 1750 treadmill, C2 BikeErg, C2 RowErg
- Barbell (45 lb) + plates (max ~270 lbs loaded)
- Dumbbells: 10, 15, 20, 30, 40, 60, 70, 80 lbs (pairs)
- Rogue rack + pull-up bar, Rogue bench, landmine attachment
- Plyo box (30/24/20"), long and short bands, jump rope
- 14 lb wall ball

**Neighbor (accessible):**
- Sled, SkiErg, sandbag, kettlebells

**Gym (15–20 min, occasional):**
- Full facility, use for race-specific prep only

---

## 3. App Redesign

### Tab Structure
**New:** Today · Coach · Trends · Plan
**Removed:** Log tab (logging moves to Coach chat + Today expandable cards)

---

### Today Tab
Default landing tab on every app open, always shows today's date.

**Contents:**
- **Today's Focus** — Santa's daily briefing: today's session from the plan, readiness assessment, key coaching note
- **Training Readiness** — HRV dot + readiness score, expandable with sleep analysis
- **Sleep Analysis** — expandable; logged via screenshot to Santa or manual entry
- **Sessions** — expandable cards; populated by Santa from chat or lightweight manual entry; shows key details (type, distance/time, notes, Santa's summary)
- **Nutrition** — macros summary
- **Weekly Summary** — visible on Sundays; Santa's weekly review

---

### Coach Tab
Central intelligence hub. All major logging and plan changes flow through here.

**Contents:**
- **Weekly summary** (top, collapsible)
- **Today's progression** — quick stats
- **Chat with Coach Santa:**
  - Auto-expanding text input
  - Voice dictation (mic button)
  - Attach multiple photos
  - Scrollable history
  - Santa's responses formatted (no tables/dividers)
- **Santa updates other tabs** — when chat contains session data, health data, or plan changes:
  - Extracts and populates Today (sessions, readiness, sleep)
  - Updates Plan (training plan modifications after confirmation)
  - Updates Trends data

---

### Trends Tab
Replaces current Progress tab. All existing data preserved.

**Fitness:**
- Sessions logged (total, by week/month)
- Runs logged (excluding Warmup-tagged)
- Miles (including Warmup miles)
- Pace trend by week/month (chart)
- Training load by week/month (volume × intensity)
- Shoe mileage tracker

**Health & Sleep:**
- HRV trend
- Resting HR trend
- Sleep score trend
- Sleep hours trend

**Nutrition:**
- Calories by week/month
- Protein, Carbs, Fat trends

All views: toggle week / month. Summary numbers + trend charts.

---

### Plan Tab
Two views, toggleable.

**View 1 — Block Overview:**
- Santa's welcome message / plan philosophy at top
- Visual calendar showing the training block (color-coded by session type)
- Block goals and focus areas

**View 2 — Session Detail (scrollable):**
- Each date labeled: Block X · Week X · [Day]
- Session details for that date
- Completed sessions show Santa's summary + key stats
- Upcoming sessions show planned workout
- Scroll forward/back through dates

**Bottom of Plan tab:**
- Goals section (editable, Santa can update)
- "Evolve Plan" button — triggers Santa to review recent performance and generate next block

---

## 4. Data Preservation

- All existing data in `S.logs`, `S.settings`, `S.summaries`, `S.trainingPlan` carried forward untouched
- New fields added to data model with safe defaults (no destructive migrations)
- Gist backup provides a restore point before any major change is deployed
- New data model additions: `S.coachChat`, `S.coachNotes`, `S.planWelcome`

---

## 5. Implementation Order

1. **Chat UX fixes** — scrollable chat, auto-expand input, voice dictation (unblocks everything else)
2. **Today tab redesign** — Today's Focus, expandable sessions, readiness layout
3. **Coach Santa system prompt** — full athlete context, memory, behavioral rules
4. **Coach → Today integration** — Santa chat populates session cards and readiness
5. **Plan tab redesign** — block overview + session detail views, welcome message
6. **Coach → Plan integration** — Santa confirms + updates training plan from chat
7. **Trends tab** — rebuild Progress into Trends with new sections
8. **Proactive coaching triggers** — Santa-initiated banners on Today tab
9. **Coach memory** — save/load coach notes from Gist
10. **Voice dictation** — mic button in chat input

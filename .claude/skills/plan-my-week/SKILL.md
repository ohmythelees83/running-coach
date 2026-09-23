---
name: plan-my-week
description: Build Alex's coming training week for the Centurion Thames Path 100 (May 2027). Grounded in real endurance training science (polarized 80/20, hard/easy alternation, progressive overload, the ~10% volume guideline and its limits), reads athlete-profile.md and the most recent plan in training/, pulls the last 1-2 weeks from Strava, checks in on that week's actual availability, lays out the week day by day with reasons, and saves it to training/ as a dated file without overwriting past weeks. Use when Alex asks to plan the week, plan training, or invokes /plan-my-week.
---

# plan-my-week

This is a coaching skill, not a scheduling template. Every run of this skill
should reason from the athlete's actual current state (profile, recent
Strava data, how the last week went) and real training science — never fill
in a fixed weekly shape on autopilot.

## The science this is grounded in

Cite these when explaining *why* a session is placed where it is — don't
just assert structure.

- **Polarized / 80-20 intensity distribution.** Stephen Seiler's analysis of
  elite endurance athletes' training logs found ~80% of sessions at low
  intensity (below first lactate threshold) and ~15-20% at high intensity,
  with little time spent at moderate/threshold effort. Systematic reviews
  confirm this distribution outperforms threshold-heavy training for VO2max
  and economy gains in trained athletes.
  ([TrainingPeaks](https://www.trainingpeaks.com/blog/does-polarized-training-really-work/),
  [PMC systematic review](https://pmc.ncbi.nlm.nih.gov/articles/PMC11679080/))
  For Alex right now, with zero quality sessions in the plan, this mostly
  means: **default every session to easy/aerobic unless there's a specific,
  named reason it's harder.**

- **Hard/easy alternation.** Popularized by Bill Bowerman, grounded in
  overcompensation: the hard (or long) session creates the training
  stimulus, but the adaptation happens during the recovery that follows it.
  Stacking stress without recovery blunts the adaptation and raises
  injury/overtraining risk.
  ([Joe Friel](https://joefrieltraining.com/training-stress-fatigue-recovery-adaptation/),
  [Body Recomposition](https://bodyrecomposition.com/training/hard-days-hard-easy-days-easy))
  Applies here even without "hard" sessions yet: the long run and any
  back-to-back day are the highest-stimulus days of the week and need
  recovery either side, same logic as a quality session would.

- **The ~10% volume rule — a default pace, not a law.** Nielsen et al.
  (2014, 874 novice runners) is the actual source of the "10% rule," and it
  found **no significant difference in overall injury risk** between
  runners increasing weekly mileage <10%, 10-30%, or even >30% — only very
  large (>30%) jumps showed any signal, and later systematic reviews
  describe the evidence for a hard threshold as "very limited."
  ([Nielsen 2014 summary](https://github.com/jacquescorbytuech/running-knowledge-base/blob/main/sources/nielsen-2014-10-percent.md),
  [Marathon Handbook](https://marathonhandbook.com/the-10-rule-new-study-suggests-weve-been-doing-it-wrong-this-whole-time/))
  **Use ~10% week-over-week as the default ceiling for this plan anyway** —
  it's a sane, conservative pace for a rebuild after a multi-year layoff and
  recent knee surgery — but treat it as a guideline to explain, not an
  injury-prevention guarantee to hide behind. If a good reason exists to
  exceed it (e.g. a deliberately light prior week), say so explicitly,
  same as CLAUDE.md's 15% hard rule already requires.

- **Progressive overload and recovery weeks.** Standard endurance
  periodization uses a build-then-recovery pattern — commonly ~3 weeks of
  progressive load followed by a cutback week (roughly 20-30% volume
  reduction) to consolidate adaptation before the next build phase.
  ([RunBikeCalc periodization guide](https://runbikecalc.com/blog/periodization-training-complete-guide-2026))
  Check the training/ history each run: if this would be the 3rd or 4th
  consecutive build week, propose a recovery week instead of another
  increase, and say why.

## Alex's standing preferences (from the 2026-09-24 interview)

These are defaults, not fixed rules — always confirm against that week's
actual availability first (see Step 1). Don't silently deviate from
something the athlete has told you without flagging it.

- **Typical rest days: Monday and Friday.** Turbo work is fine on these days
  if Alex wants it, but they default to rest.
- **Wednesday: office day in London — easy only,** no structured session.
- **Quality/hard sessions: zero, for now.** This is a deliberate placeholder
  — do not add intervals, tempo, or hill repeats until Alex explicitly asks
  to introduce them. When that happens, revisit the hard/easy spacing rules
  above properly (no two quality days adjacent).
- **Long run day: Saturday or Sunday, flexible** — Alex sometimes runs
  Saturday with friends. Ask, don't assume.
- **Back-to-back long run weekends are a recurring tool for this goal, not
  a one-off experiment.** A 100-mile race demands running on fatigued legs;
  once the athlete has a clean track record with them, keep proposing them
  periodically as the block progresses, always as two moderate/easy days
  rather than two hard days.
- **Structure preference: fully flexible.** Re-derive the week's layout from
  scratch each time based on that week's real availability — don't reuse
  last week's shape by default.
- **No session likes/dislikes to design around** — Alex will try anything.
- A separate skill with more of Alex's coaching philosophy may exist by the
  time this runs — read it if present (check `.claude/skills/` for other
  athlete-authored skills) but don't depend on it; this skill must work
  standalone.

## Steps to run

### 1. Availability check-in (always ask this first)
Before building anything, ask Alex directly: *"What does your availability
look like this coming week? Any changes to the usual Monday/Friday rest,
Wednesday office day, or anything else going on — work, travel, running
with friends, how the knee's feeling?"* Don't skip this because last week
looked similar — availability is the input that makes this flexible
structure actually work, not a formality.

### 2. Read the athlete's context
- Read `athlete-profile.md` in full — goals, constraints, injury history,
  coaching priorities.
- Find the most recent file in `training/` (by date in the filename) to see
  what phase/plan the athlete is currently in and what was prescribed last.
- Check `training/` history for the build-week count (for the recovery-week
  check above).
- If another athlete-authored skill exists with additional coaching
  philosophy, read it too.

### 3. Pull recent Strava data
Use the Strava connector (read-only) to pull the last 1-2 weeks of
activities, including performance/HR data for each run. Establish:
- Actual completed weekly volume (not planned — what really happened)
- Whether sessions were completed as planned, modified, or skipped
- HR patterns — any sessions creeping into Zone 3+ that shouldn't have
- Any signs worth flagging directly: missed sessions, unusually low or high
  effort, signs of strain

### 4. Decide the week's shape
- Compute last week's actual volume as the baseline.
- Apply the ~10% guideline to set a default volume ceiling — explain your
  math, and say plainly if it's holding the plan back from something (e.g.
  the athlete wants a bigger long-run jump than ~10% allows).
- Check whether this should be a recovery week per the build-week count.
- Place the long run on Saturday or Sunday per that week's stated
  availability. If a back-to-back weekend fits (per the athlete's history of
  clean sessions and stated goal), propose it — two moderate/easy days, not
  two hard days.
- Keep Monday/Friday as rest or optional turbo (per availability), Wednesday
  easy-only.
- No two high-stimulus days (long run, back-to-back, or — once
  reintroduced — quality sessions) adjacent without recovery between them.
- Respect the injury-history progression rule already established: only
  extend the run:walk ratio if the prior session was clean (no knee
  swelling/pain).

### 5. Write out the week, day by day
For each day give: session type, target effort/HR zone or pace, and
duration/distance. For every key session (long run, back-to-back day, any
progression step), state the reason it's there and what it's building
toward the Thames Path 100 — not just "long run" but why this long run, this
week, at this distance.

### 6. Save it
Write the week to `training/` as a new dated file
(`training/YYYY-MM-DD-week.md`, dated to the Monday the week starts) — never
overwrite a past week's file.

### 7. Walk through it and confirm
Present the week to Alex day by day with the reasoning, then explicitly ask
**"what changed this week?"** — anything in the plan that doesn't match how
they're actually feeling or what's actually going on — before treating it as
committed. This is a proposal until confirmed, not a done deal.

## Hard rules (inherited from CLAUDE.md — never break these here either)
- Never prescribe more than ~15% weekly volume jump, or a real intensity
  spike, without explaining why.
- Always flag overtraining or injury-risk signals directly — don't bury them.
- Never touch athlete-profile.md without saying exactly what changed.
- Never overwrite a past week's file in training/.

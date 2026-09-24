---
name: update-dashboard
description: Refresh Alex's Athlete OS dashboard (dashboard.html and its linked hosted copy) with current numbers after a session is logged, or whenever asked to update/refresh the dashboard. Pulls fresh Strava data, recomputes ACWR/volume/readiness/paces, reconciles this week's planned sessions against what's actually been completed, and republishes both the local file and the hosted Artifact link in place. Use when Alex says a session is done, asks to refresh/update the dashboard, or invokes /update-dashboard.
---

# update-dashboard

Regenerates the whole dashboard from current data rather than patching it
piecemeal — full regeneration avoids drift between numbers. This is not a
weekly-planning skill (that's `plan-my-week`); it only reflects reality
back, it never changes the plan itself.

## Where things live
- Local file: `dashboard.html` at the project root — the canonical,
  double-click-to-open version. Always update this.
- Hosted copy: the same content published as a Claude Artifact. The URL is
  recorded in `athlete-os.md` under Status — read it from there, don't ask
  the user for it. Update the *same* artifact (pass its `url`) so the link
  Alex already has keeps working; never publish a fresh one unless that URL
  is missing or broken.
- Design reference: `dashboard.html`'s own `<style>` block is the source of
  truth for the palette (Thames/Centurion-inspired: river blue `#0b7fae` /
  `#379fc9` dark, crimson `#a23b32` / `#d1685c` dark, warm stone surfaces)
  and layout. Keep using it — don't redesign on a refresh.

## Steps

### 1. Read current state
- `athlete-profile.md` — goal race date, Coaching Philosophy (for the
  insights panel), current HR zones.
- The most recent file in `training/` (by date in the filename) — this
  week's planned sessions, the HR targets, the progression rule.
- `.claude/skills/` — list skills present (for the skill tiles), in case
  one's been added or removed since the last refresh.

### 2. Pull fresh Strava data
- `list_activities` for the last ~9 weeks (covers the 4-week ACWR window,
  a 6-week volume trend, and enough history to catch the return-to-running
  milestone if it's still in range).
- Filter to `Run` and `TrailRun` sport types for volume/pace; keep all
  activities (including Walk/VirtualRide) for the ACWR load calculation,
  using `relative_effort` as the load unit.

### 3. Recompute every metric — don't reuse last time's numbers
- **Race countdown:** days from today to the goal race date.
- **ACWR:** sum of `relative_effort` over the last 7 days, divided by the
  average weekly `relative_effort` over the last 28 days. Badge it:
  - 0.8–1.3 → "In range" (good)
  - <0.8 → "Fresh / low load" (good, unless it's been low for several
    consecutive weeks — then say so instead of defaulting to "good")
  - 1.3–1.5 → "Elevated" (warning)
  - >1.5 → "High — worth a look" (serious)
- **This week's running:** actual mi logged so far this Mon–Sun week vs.
  the plan's target for the week.
- **3-week avg volume:** mean of the last 3 full Mon–Sun weeks' Run +
  TrailRun mileage.
- **Days since last run**, and its badge (well rested / fine / flag if
  it's been a long gap unexpectedly).
- **Recent run sessions table:** last 5 runs — date, distance, pace,
  avg HR.
- **Weekly volume trend chart:** last 6 Mon–Sun weeks, Run + TrailRun
  mileage per week. Mark the current week `inProgress: true` if it hasn't
  ended yet (reuse the existing dashed-bar treatment in the chart JS).

### 4. Reconcile this week's session list against what actually happened
This is the core of "update once a session is done": for each day in the
current week's plan (from the training/ file) up to and including today,
check whether a matching Strava activity exists near that date.
- **Completed as planned or close to it:** mark done — replace the target
  with the actual (e.g. "✓ 4.6mi actual" instead of "~4.5mi target"), swap
  the tag to a neutral "Done" style (reuse `.session-tag.rest`'s muted
  look, not the river/crimson accent — accent color is for what still
  needs attention).
- **Skipped (no matching activity, day has passed):** mark "Skipped",
  same treatment as the existing Wed-23 example in the current file.
- **Meaningfully different from plan** (e.g. distance far off, or it was
  supposed to be a progression step and the run:walk ratio in the data
  doesn't match): still mark done, but note the difference plainly rather
  than silently overwriting the plan's intent — e.g. "Done — 3.2mi
  (shorter than planned)".
- **Today:** keep the `.today` highlight treatment if today's session
  hasn't happened yet; if it has (matching Strava activity found), treat
  it like any other completed day instead.
- **Future days in the week:** unchanged, still show the plan's target.

### 5. Update the readiness call
Rebuild the "Today's Readiness Call" card from the fresh numbers (days
since last run, ACWR, and — since `health/` may still be empty — keep the
honest note that this is inferred from training data only, not logged
symptoms, unless `health/` now has entries to draw on directly).

### 6. Refresh the coaching panel
Keep the same structure (overview, 4 insights each tied to a Coaching
Philosophy section or a named framework, tips for right now) but regenerate
the content from what the fresh data actually shows — don't just carry
forward last time's insights if they're no longer the most relevant ones.
If a completed session (especially the back-to-back or a progression step)
produced a notable result, that belongs in an insight or a tip.

### 7. Write both files
- Overwrite `dashboard.html` in full with the regenerated page.
- Build the artifact-ready version (same content, without the
  `<!DOCTYPE>`/`<html>`/`<head>`/`<body>` wrapper tags — the Artifact
  publish skeleton supplies those) to a temp file in this session's
  scratchpad directory, then publish it with `action: "publish"` and the
  stored `url` from `athlete-os.md` so it updates the same link in place.

### 8. Commit and push
Stage and commit `dashboard.html` (and `athlete-os.md` if the recorded URL
changed) with a message naming what changed since the last refresh — don't
just say "update dashboard." Push to the working branch.

### 9. Tell Alex what changed
Short summary: which sessions got marked done, any metric that moved
meaningfully (ACWR, volume trend), and anything the coaching panel is now
flagging that it wasn't before. Don't just say "refreshed" — say what's
different.

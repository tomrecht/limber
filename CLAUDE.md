# Limber – daily exercise checklist

Personal PWA, live at https://tomrecht.github.io/limber/ – GitHub Pages serves `main` as-is, so a push to `main` is a deploy. No build step.
The owner uses it from their phone. Keep replies short and say plainly what changed.

- `index.html` – the whole app (CSS, HTML, one vanilla-JS `<script>`, no dependencies).
- `sw.js` – offline cache: page network-first (4 s timeout), the rest cache-first. **Bump `CACHE` (`limber-vN`) on every deploy.**
  It deletes only `limber-*` caches: the origin tomrecht.github.io is shared with Woodshed (`woodshed-*` caches), whose worker does the same.
- Data: `localStorage` key `limber.v1` (shared origin, so it carried over from the old /woodshed/limber/ address).
  `ex` = exercises `{id, name, kind: 'reps'|'time', target, sets, sides, note}`;
  `log[day] = {d: {exId: sets done}, v: {exId: [entry per set]}, n, full}`, entry = `{n}` (reps or seconds) or `{l, r}` for each-side, `rec: 1` = beat
  every earlier day (each-side counts by the weaker side). Days logged before 2026-11-07 have `d` only (plain ticks); keep them working.
- Logging: tapping an exercise opens a number entry for one set (prefilled from that set last time); chips edit/delete; extra sets allowed.
  The timer records seconds actually held. A hold never ends by itself: at the target it chimes and counts overtime (+N, chime again on
  passing the best) until "Next side/set →" or "Done" (Done mid-hold counts the time so far; one side only → entry sheet for the other).
  Only today's `full` is recomputed, so editing the list never rewrites past days. Never lose the owner's data: migrate, don't reset.
- Game (2026-11-07), all DERIVED from the log except per-exercise `lvl`/`target`: XP per day (`dayXp`, values in `XP`) + weekly goals
  met (`S.goal`, default 5 full days Mon–Sun) + badges; player level `levelOf` (n→n+1 costs 100+50(n−1)). Rising targets (`checkLevelUp`):
  every set at target on 3 logged days running → target +1 rep / +5 s, `lvl`+1, `log[day].up[id]` = old target (undo reverts); `x.auto === false`
  turns it off; `log[day].t[id]` = target when logged. Streak freezes (`sim`): 1 per 7 full days in a streak, max 2, a missed day uses one
  (shown ❄ on the calendar). Daily challenge `log[day].ch` (seeded by the date, `ensureChallenge`). Badges (`badges()`): series of thresholds;
  `S.seen` = announced ids, `S.lvlSeen` = announced level. Pop-ups queue (`celebrate`). Progress sheet: tap the level bar.
- Timed exercises: timer with 5 s get-ready (auto), each side, sets; wake lock, beeps, vibration.
- Check a change: serve with `python3 -m http.server`, drive it in headless Chromium at ~380 px wide, check the console for errors.

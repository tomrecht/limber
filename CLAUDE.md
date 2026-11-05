# Limber – daily exercise checklist

Personal PWA, live at https://tomrecht.github.io/limber/ – GitHub Pages serves `main` as-is, so a push to `main` is a deploy. No build step.
The owner uses it from their phone. Keep replies short and say plainly what changed.

- `index.html` – the whole app (CSS, HTML, one vanilla-JS `<script>`, no dependencies).
- `sw.js` – offline cache: page network-first (4 s timeout), the rest cache-first. **Bump `CACHE` (`limber-vN`) on every deploy.**
  It deletes only `limber-*` caches: the origin tomrecht.github.io is shared with Woodshed (`woodshed-*` caches), whose worker does the same.
- Data: `localStorage` key `limber.v1` (shared origin, so it carried over from the old /woodshed/limber/ address).
  `ex` = exercises `{id, name, kind: 'reps'|'time', target, sets, sides, note}`; `log[day] = {d: {exId: sets done}, n, full}`.
  Only today's `full` is recomputed, so editing the list never rewrites past days. Never lose the owner's data: migrate, don't reset.
- Timed exercises: timer with 5 s get-ready, each side, sets; wake lock, beeps, vibration.
- Check a change: serve with `python3 -m http.server`, drive it in headless Chromium at ~380 px wide, check the console for errors.

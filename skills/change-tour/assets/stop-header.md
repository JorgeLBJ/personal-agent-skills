# Change Tour Templates

Translate labels to the user's language. Progress bar: 10 cells total, filled `█` = round(current/total*10), empty `░`.

## Tour index (first message)

```
╔══════════════════════════════════════════════════╗
║  🧭  CHANGE TOUR · {feature name}                ║
║  {total} stops · {n} files changed               ║
╚══════════════════════════════════════════════════╝
```

| # | Stop | Route / Screen |
|---|------|----------------|
| 1 | {title} | `{route}` |

## Stop header

```
╔══════════════════════════════════════════════════╗
║  📍 STOP {current} / {total} · {stop title}      ║
║  {██████░░░░} {percent}%                         ║
╚══════════════════════════════════════════════════╝
```

Under the box, a breadcrumb: `✅ 1 · ✅ 2 · 📍 3 · ⬜ 4 · ⬜ 5`

## Waiting footer

```
──────────────────────────────────────────────────
⏸  Waiting for you · next: STOP {current+1} — {next title}
   Reply "next" to continue, or ask anything about this stop.
──────────────────────────────────────────────────
```

On the last stop, replace "next: ..." with "next: tour summary".

## Closing summary

```
╔══════════════════════════════════════════════════╗
║  🏁  TOUR COMPLETE · {total}/{total} stops        ║
║  {██████████} 100%                               ║
╚══════════════════════════════════════════════════╝
```

- ✅ Stop 1 — {title}
- 📝 Notes raised during the tour: {list or "none"}

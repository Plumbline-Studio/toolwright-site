## Plumbline dashboard — session ritual

This repo is a tracked project on dashboard.toolwright.dev — the studio's memory spine. Keep it in step; this is your job as the assistant, not something Kyle has to remember.

**At session start**, surface where this project was left off, and offer it before doing new work:
- If the Supabase connector is available, read the last few notes and any open loose ends:
  `select summary, next_action, kind, occurred_at from session_notes where repo_id = (select id from repos where name = 'toolwright-site') order by occurred_at desc limit 5;`
  and list open GitHub issues labeled `loose-end` on this repo.
- If the connector is not available, read the last lines of `.plumbline/journal.jsonl` instead.

**At session end, and at any pivot, merge, or decision**, log a note so nothing is lost across sessions or devices. Do this automatically.
- Preferred — via the Supabase connector (works from phone and desktop, Code or Cowork):
  `select add_session_note(p_repo => 'toolwright-site', p_summary => 'what changed', p_next_action => 'where to pick up', p_kind => 'progress', p_surface => 'code');`
  Use `p_kind` of `pivot`, `merge`, or `decision` when apt. Add `p_stage => 'building'` (one of scaffold|building|demo_ready|live|paused|archived) ONLY when the project's stage actually changed.
- Fallback — if no connector, append one JSON line to `.plumbline/journal.jsonl` and commit it:
  `{"ts":"<ISO8601>","kind":"progress","summary":"...","next_action":"...","surface":"code","commit":"<sha>"}`
  The dashboard's next GitHub sync ingests it.

Supabase project: `ghddsckqbwrjsjvbjwya`. `add_session_note` stamps the owner and resolves the project from the repo automatically, so `p_repo => 'toolwright-site'` is enough.

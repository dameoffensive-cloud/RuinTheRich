# Crew sync setup (shared billionaire health for group play)

Without this, crew mode = everyone plays the same seeded market separately
and compares scores. With it, crew mode becomes a **raid**: the four
billionaires' net worth is one shared health pool, every crew member's damage
counts toward it, and you see teammates' hits arrive in your log (👯 +$412).
Personal cash stays personal by design — shared wallets across devices cause
"we both spent the same money" conflicts that need a real game server.

## Setup (~5 minutes, free)

1. Create a free account at **supabase.com** → New project (any name/region).
2. In the project: **SQL Editor** → paste and run:

```sql
create table crew_events (
  id bigint generated always as identity primary key,
  day int not null,
  crew text not null,
  player text not null,
  bil int not null,
  dmg numeric not null,
  created_at timestamptz default now()
);
create index on crew_events (day, crew);
alter table crew_events enable row level security;
create policy "anon can insert" on crew_events for insert to anon with check (true);
create policy "anon can read"  on crew_events for select to anon using (true);
```

3. **Project Settings → API**: copy the *Project URL* and the *anon public* key.
4. In `index.html`, find and fill:

```js
const SB_URL='';   // → 'https://yourproject.supabase.co'
const SB_KEY='';   // → the long anon key
```

5. Re-upload `index.html` (and bump the version in `sw.js`). Done — any crew
   run now syncs. A ⚡ appears next to the crew name in the header when sync
   is active.

## How it behaves

- The shared pool starts at $2,500 per billionaire at UTC midnight and grows
  50% over the day (billionairism, but deterministic so every device agrees).
- Your hits are batched and uploaded every 4 seconds; everyone polls every 6.
  Expect a few seconds of lag between a teammate's dump and your bar moving.
- A billionaire dies when the shared pool hits zero — for the whole crew.
- Buying from billionaires does NOT refill the pool in raid mode (otherwise
  one confused teammate could heal the boss).
- Solo daily and free play are untouched — sync only activates for crew runs.

## Honest limitations (fine for a beta)

- The anon key is public (it's in the page source). Anyone can post fake
  damage rows — trivial to cheat. Acceptable among friends; a real launch
  would validate damage server-side (Supabase Edge Function).
- Old rows accumulate. Occasionally clear them in SQL Editor:
  `delete from crew_events where created_at < now() - interval '2 days';`
- Supabase free tier pauses after a week of inactivity — just wake it in the
  dashboard.

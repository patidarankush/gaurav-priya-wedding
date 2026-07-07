# Gaurav & Priya — Wedding Invitation

A mobile-first, scroll-animated Indian wedding invitation. Tap-to-open intro,
themed scroll story for each celebration (Haldi · Sufi Night · Baraat & Phere ·
Reception), live countdown, looping background music, and an RSVP form that saves
to Supabase.

Everything is **one self-contained file** (`index.html`) plus an `assets/` folder.
No build step, no framework — just drag the folder onto any static host.

```
invite-new/
├── index.html          ← the whole website
└── assets/
    ├── hero-*.webp      ← event illustrations (couple, haldi, sufi, baraat, reception)
    ├── intro-gate.webp  ← cover picture
    ├── ganesha.webp     ← Ganesha motif
    ├── garland / lantern / diyas / floral-corner / dervish .webp  ← decor cutouts
    └── PUT-YOUR-MUSIC-HERE.txt
```

---

## 1) Background music  (1 minute)

Drop **one MP3** into the `assets/` folder named exactly:

```
assets/wedding-music.mp3
```

It loops automatically and starts on the first **“Tap to open”** tap (browsers
block audio before a tap). The floating ♪ button mutes / unmutes. If the file is
absent, the site still works — just silently.

Free, licence-safe tracks: **Pixabay Music**, **YouTube Audio Library**,
**Bensound**, **Uppbeat**. Search e.g. *“sitar”*, *“shehnai”*, *“Indian wedding
instrumental”*. If you host a copyrighted Bollywood song publicly, that copyright
is your responsibility.

---

## 2) RSVP → Supabase  (about 3 minutes, free tier)

The form already works out of the box — until you add keys, responses are saved
in the visitor’s own browser (localStorage) so nothing breaks. To collect
responses centrally:

1. Create a free project at **https://supabase.com**.
2. Open **SQL Editor** and run:

   ```sql
   create table rsvps (
     id uuid primary key default gen_random_uuid(),
     created_at timestamptz default now(),
     name text not null,
     phone text,
     attending text,
     side text
   );

   -- Let the public invitation submit (insert only), nothing else:
   alter table rsvps enable row level security;
   create policy "anon can insert rsvps"
     on rsvps for insert to anon with check (true);
   ```

3. In **Project Settings → API**, copy the **Project URL** and the
   **anon public** key.
4. Open `index.html`, find this block near the bottom (in the `<script>`) and
   paste them in:

   ```js
   var SUPABASE_URL      = "https://YOUR-PROJECT.supabase.co";
   var SUPABASE_ANON_KEY = "YOUR-ANON-KEY";
   ```

5. Save and re-deploy.

**See responses:** Supabase → **Table Editor → `rsvps`** (or *Export → CSV*).

> Only the anon **insert** policy is enabled, so the public key is safe to ship —
> no visitor can read the guest list from the page. A hidden honeypot field
> quietly drops bot submissions.

The form collects: **name, phone, attending (yes/no), and groom/bride side.**

---

## 3) Free hosting  (drag-and-drop, generous free tiers)

Deploy `index.html` **and** the `assets/` folder together.

| Host | How | Notes |
|------|-----|-------|
| **Netlify Drop** | https://app.netlify.com/drop — drag the whole `invite-new` folder in | Easiest. Instant public link. |
| **Cloudflare Pages** | https://pages.cloudflare.com — drag-and-drop or Git | Very fast global CDN. |
| **Vercel** | https://vercel.com — drag-and-drop or Git | |
| **GitHub Pages** | Commit files to a repo → Settings → Pages | Free custom-domain support. |

`index.html` is the default entry file every host serves automatically. Add a
custom domain (e.g. `gauravweds priya.com`) later from the host’s dashboard.

---

## 4) Editing later

Open `index.html` in any text editor — all names, dates, venues, dress code and
the invitation text are plain text you can change directly. A few handy anchors:

- **Countdown target** — search `TARGET =` (set to the Haldi start, 22 Jul 2026 13:30 IST).
- **RSVP deadline** — search `Kindly respond by`.
- **Map links** — search `google.com/maps` (currently point to “Simcha Island”).
- **Music file** — `assets/wedding-music.mp3`.
- **Fonts** — Fraunces (display), Jost (labels), Tiro Devanagari (Hindi), loaded from
  Google Fonts in the `<head>`.
- **Colours per event** — the `.ev-haldi`, `.ev-sufi`, `.ev-baraat`,
  `.ev-reception` theme rules near the top of the `<style>` (each sets its
  accent + gradient via CSS variables).

Want a different image? Replace any file in `assets/` (keep the same filename), or
regenerate it and drop it in.

---

*Made with love — ॥ जय झूलेलाल ॥*

# PULSE — running the demo

Two HTML files in this folder:

- **`pulse.html`** — public marketplace
- **`pulse-admin.html`** — admin dashboard (CMS + analytics)

They share data via `localStorage`. When you save an artist in admin, the
public site picks it up and re-renders live. Drafts are hidden from the
public site automatically.

## Recommended: serve via local web server

Browsers (Chrome/Edge especially) treat each `file://` URL as its own origin,
which means `localStorage` and `BroadcastChannel` are NOT shared between the
two files when you double-click them. To get real cross-tab sync, serve them
both from the same origin:

```bash
# In this folder:
python3 -m http.server 8000
# or:
npx serve .
```

Then open:

- http://localhost:8000/pulse.html
- http://localhost:8000/pulse-admin.html

Now any save you make in admin appears immediately on the public site
(in another tab).

## Or: open both files directly

If you just double-click the HTML files:

- Each app works independently with its own seed data.
- In **Firefox**, files in the same folder share `localStorage`, so admin
  edits do propagate to the public site.
- In **Chrome/Edge**, the public site falls back to bundled seed data; admin
  edits won't be visible there.

## Cross-links

- The public site footer has a small **Admin** link → opens `pulse-admin.html`.
- The admin sidebar has an **↗** icon next to the logo → opens `pulse.html`.

## Login (admin)

Any email + any password works. Auth is stored in `localStorage`. Click
**Sign out** in the bottom-left of the sidebar to leave.

# Overlap — find days that work for everyone

One dark, minimal page where friends paint the days they're free and the group's overlap lights up green. Live: everyone sees everyone's changes in under a second.

**Try it right now:** double-click `index.html`. Until Firebase is connected it runs in demo mode — local to your device, with three sample friends so the heatmap has something to show.

## Using it

- Every trip gets its own link: `yoursite/?e=camping-july`. Visiting a fresh link creates the event; the landing screen just asks your name (remembered per device).
- **Tap** a day to toggle it. **Drag** to paint a stretch. **Hold** (or right-click) a day to set a time — all day / morning / afternoon / evening — add a note like "after 5pm", and see who else is free.
- One shared color, and it simply **deepens as more people are free**: one person is a whisper, half the group glows with a tinted border and halo, and a **whole-group day** gets a ring with a slow breathing glow. One glance shows the days that work. The count sits bottom-right. "Best days" below the calendar ranks the top overlaps — tap one to jump to it.
- **Spotlight a person**: tap a name in "Who's in" to show only their days, with their times. Tap again — or press Esc — to show everyone.
- **Lock the date**: the crown on a best-day chip (or in any day's popover) locks the winner for the whole group — everyone sees a banner with one-tap **Add to calendar** (.ics). The ✕ unlocks if plans change.
- **Live presence**: while a friend is picking, a soft glowing dot with their name trails across the days they're touching — you can watch the plan form.
- **Weekends**: one tap under the calendar paints all your weekends in the visible month; tap again to clear them.
- **Themes**: the droplet icon toggles six looks — **Frost** (liquid-glass, floating, the default), Basalt (sage), Ember (campfire amber, serif), Glacier (alpine ice), Darkroom (monochrome print), Dusk (desert violet). Choice is remembered per device.
- **Joining**: there are no invites or accounts — the link is the invite. Anyone who opens it types their name once and is instantly on the calendar for everyone.

## Going live (one-time setup, ~15 min, free)

### 1. Firebase — the live sync

1. Go to [console.firebase.google.com](https://console.firebase.google.com) → **Add project** (any name, turn Analytics off).
2. Left menu: **Build → Realtime Database → Create database** → pick a US region → **Start in locked mode**.
3. Open the **Rules** tab, replace everything with the JSON below, hit **Publish**:

```json
{
  "rules": {
    "events": {
      "$event": {
        ".read": true,
        ".write": true
      }
    }
  }
}
```

4. Project **⚙️ Settings → General → Your apps → Web app (`</>`)** → register it (skip hosting) → copy the `firebaseConfig` object it shows.
5. Open `index.html` in a text editor, find this line near the top of the script:

```js
const FIREBASE_CONFIG = null;
```

Replace `null` with the copied object. **Make sure it includes `databaseURL`** — if it doesn't, add it yourself from the Realtime Database page (it looks like `https://YOURAPP-default-rtdb.firebaseio.com`).

### 2. GitHub Pages — the hosting

1. On GitHub, create a new **public** repo (e.g. `overlap`).
2. Upload `index.html` to the repo root.
3. **Settings → Pages** → Source: *Deploy from a branch* → `main` / `/ (root)` → Save.
4. First build can take 5–10 minutes. Your site lives at `https://YOURUSERNAME.github.io/overlap/`.

### 3. Share

Open your site, name a trip, hit the share icon — it copies the `?e=trip-name` link. Anyone with the link can mark their days. No accounts, no installs.

## Notes

- **Trust model:** like When2meet — anyone holding an event link can edit that event. Keep links among friends; don't put private info in notes.
- People are identified by the name they type. Same name on phone + laptop = same person (that's a feature).
- Firebase's free tier handles a friend group's scheduling roughly forever. No card required.

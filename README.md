# Vireo · The Track to the Final — World Cup 2026 Sweepstake

A single-page prediction game in Vireo's rail-network style. 48 teams board 12 group
platforms, switch tracks at the knockout junctions, and converge on one terminus — the Final.

- **Predictions** per player (Mo / Thea / Lucas / Jan)
- **Admin** enters real results and fills knockout junctions
- **Kicktipp scoring** — exact **5**, goal difference **3**, correct result **2**
- **Furthest-underdog bonus** — stage reached × FIFA-ranking weight (lowlier = more); champion pick **+10**
- Live **Signal Board** leaderboard, live group standings, live bracket

The pool scores from fixture **#33** onward (earlier results are locked and only drive the standings).

## Shared live leaderboard (Firebase) — one-time setup, ~5 minutes

Until you add a Firebase config the app runs **local-only** (each device keeps its own picks;
merge with the Export / Import buttons). To make all four of you share one live board:

1. Go to <https://console.firebase.google.com> → **Add project** (any name, e.g. `vireo-sweepstake`). Disable Analytics — not needed.
2. In the project: **Build → Firestore Database → Create database** → start in **production mode** → pick a region.
3. **Build → Authentication → Get started → Sign-in method → Anonymous → Enable.**
4. Project settings (gear icon) → **Your apps → Web (`</>`)** → register an app → copy the `firebaseConfig` object.
5. Paste those values into [`firebase-config.js`](firebase-config.js) (replace the `PASTE_…` placeholders) and commit.
6. Firestore → **Rules** → paste and publish:

   ```
   rules_version = '2';
   service cloud.firestore {
     match /databases/{database}/documents {
       match /pools/{pool} {
         allow read, write: if request.auth != null;
       }
     }
   }
   ```

Reload the site — the pill in the toolbar should read **“Shared sync live.”** Everyone now
reads and writes one shared pool (`pools/default`). Each writer only touches their own slice,
so simultaneous edits don't clobber each other.

> The Firebase config is public by design; the Anonymous-auth rule above just keeps random
> drive-by writes out. It's a friendly office pool, not a bank.

## Local / offline use

Open `index.html` directly (double-click) — it works fully offline in local-only mode.
Built from the Vireo brand kit; same HTML → print-to-PDF system as the other templates.

# morning-routine

A guided morning ritual: a single-page app that walks through 10 steps, one screen at a time.

**Live:** https://morning.sadhanas.app (Cloudflare Pages, behind Cloudflare Access) and https://dicaguca.github.io/morning-routine/ (GitHub Pages). Both deploy from `main`.

## Steps

1. Greet Your Shiny Sunny 🌞
2. Waking Up Moment (opens Waking Up)
3. Daily Check-In (opens habits.sadhanas.app)
4. Mantras (built-in notes editor)
5. Lessons & Wisdom (built-in notes editor)
6. Core Commitments (opens a ChatGPT project chat)
7. D RAM 2025 Review
8. PDM Journaling
9. Check In with Sunny 🌞
10. Start Workday with Stru (opens stru.sadhanas.app)

Each screen has its own URL: `#/welcome`, `#/step1` … `#/step10`, `#/complete`.

## How it's built

- Everything is in `index.html`: React 18 and Tailwind from CDNs, with JSX compiled in the browser by Babel standalone.
- Babel is pinned to `7.23.9` and the script uses the classic JSX runtime pragma. Newer Babel versions broke the page, so don't unpin it.
- Mantras and Lessons notes save to the browser's `localStorage` (`morningRitualMantrasContent`, `morningRitualLessonsContent`) as you type, and about a second later to the sadhanas API (`https://api.sadhanas.app/morning-mantras` and `/morning-lessons`, Cloudflare Worker + KV) as `{ html, updatedAt }`. When a note opens, whichever copy was edited last wins; a browser copy replaced by a newer cloud copy is kept under `<key>PreviousLocal`. The API only accepts requests from the origins listed in its `ALLOWED_ORIGINS`, which includes `https://morning.sadhanas.app` but not GitHub Pages, so cloud sync works on the sadhanas.app address only.

## Making changes

- Step titles, descriptions and links are in the `ritualSteps` list in `index.html`.
- Commit to `main` and push. Cloudflare Pages and GitHub Pages both republish automatically.
- Sister app: [shutdown-routine](https://github.com/dicaguca/shutdown-routine).

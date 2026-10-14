# Your Financiën site

A 6-page site:

- **`fin-88yihnu9.html`** — the landing page, with tiles linking to the other five
- **`budget.html`** — the joint/personal budget dashboard with Maarten and Nicolas
- **`portfolio.html`** — your investment portfolio (fetches live prices in-browser)
- **`networth.html`** — assets vs. liabilities, pulled together into one net worth figure
- **`house.html`** — your mortgage, broken down by loan part, plus a house value vs. equity donut
- **`new_house.html`** — the potential-next-house planner: required savings goal with a
  moving/buying-cost checklist, a full budget simulation for what life looks like once you
  own it, a mortgage what-if calculator (purchase price → equity + carried-over mortgage +
  new mortgage, with an estimated monthly payment and hypotheekrenteaftrek), and a step-by-step
  moving checklist with locked stages

Five of the six pages (everything except `new_house.html`) are fully local — no login, no
database, nothing sent anywhere. **`new_house.html` is the exception**: it syncs its data to
a shared Supabase database, so you and Nicolas can both open it and see the same numbers, with
no sign-in needed. See "About new_house.html's cloud sync" below.

All six share the same look — fonts, colors, header style, and card design — so they read
as one product. Keep all six files together in the same folder; the landing page links to
the others by filename, so if you rename any of them you'll need to update the links in
`fin-88yihnu9.html` to match.

## About new_house.html's cloud sync

Unlike the other five pages, `new_house.html` opens straight away — no sign-in — and its four
tabs (🏦 Mortgage, 🧮 Budget plan, 💰 Required savings, ✅ Checklist) all read from and write to
a shared Supabase table (`budget_dashboard_state`, row `scenario: 'b'`), so whichever of you
opens it sees the other's latest saved numbers — not just what's baked into the file. Nothing
autosaves here either: edits stay in memory until you hit **💾 Save**, and **↺ Undo changes**
discards unsaved edits and reloads what's actually in the database. It also reads
`scenario: 'a'` (your Current Budget) read-only, purely to show "vs. today" comparison
figures — it never writes to that row.

If this page ever can't load/save, the Supabase project or its credentials may have changed
since this was last set up — let me know and I can help re-point it.

## Opening it on your phone

If you tap one of these files on a phone, some apps (iOS's Quick Look preview, for instance)
show a preview of the page without actually running its JavaScript — everything will look
static and any live editing/recalculating won't work. To be safe, use Share → **Open in
Safari** (or **Open with Chrome** on Android) instead of just tapping the file, so it opens
in a real browser tab. Every page's starting numbers (except `new_house.html`, which loads
from the database instead) are also baked directly into the file itself, so even a
JavaScript-less preview will show today's real figures rather than blank zeros — you just
won't be able to edit anything until it's open in an actual browser.

## Using it now

Just open `fin-88yihnu9.html` in your browser — no setup needed for five of the six pages:

- The budget dashboard, net worth page, and home & mortgage page all have **Export**,
  **Import**, and **Reset** buttons for manually backing up or restoring your edited numbers.
- The portfolio page fetches live prices each time you load it — nothing to save there.
- `new_house.html` works differently — see "About new_house.html's cloud sync" above. It has
  its own **Export** (download a snapshot), **💾 Save** (write to the shared database), and
  **↺ Undo changes** (reload the last saved values) buttons.

## About the "Sync" buttons

**Net Worth** has a **↻ Sync from Portfolio & Home** button that pulls several things in
automatically, so you don't have to retype them:

- Your investments and savings split, from `portfolio.html`
- Mortgage loan parts 1.0 and 1.1's remaining balances, from `house.html`

**new_house.html**'s Mortgage tab has a **↻ Sync equity & current mortgage from Home &
Mortgage** button that pulls your current house's equity and remaining mortgage balance in
from `house.html`.

One important thing to know: these pull whatever is **currently saved in those files**, not
whatever you happen to have typed into another open browser tab. Since `house.html` and
`portfolio.html` don't save your edits anywhere, if you've been editing one in another tab,
that in-progress data isn't accessible to the sync — only what's baked into the file itself.
In practice this means: after your mortgage figures update, your portfolio changes, or I
update the relevant file for you, hit **Sync** on the relevant page to pull the new figures
in one click, instead of retyping them.

Savings, other assets/debts, the credit card row, and other manual liabilities on the Net
Worth page stay manual — nothing tracks those elsewhere on the site. House value is a special
case: it's entered separately in two places on purpose — once on the Net Worth page (used for
your overall net worth figure) and once on the Home & Mortgage page (used for that page's own
equity donut chart). Sync doesn't connect the two, so if you update one, update the other too.

On new_house.html's Mortgage tab, the new mortgage's interest rate is adjustable (▲/▼
buttons) but the buying-costs percentage and term stay fixed estimates — only you (or your
mortgage advisor) know what a bank would actually offer for a specific new house.

## Keeping the mortgage figures current

`house.html`'s loan parts now advance automatically: each Remaining/Paid off figure moves
forward by one annuity payment for every calendar month that's passed since it was last set,
recalculated fresh every time you open the page — so you don't need to check your bank app
each month just to keep this in sync. This assumes a fixed rate and no extra/early
repayments; if a loan part's "Rate valid until" date passes, the page will show a warning
under Loan parts, and you'll need to update the rate by hand once your bank tells you the new
one (the automatic projection keeps using the old rate until then). If you ever want to
correct a figure against what your bank actually shows, just type over the Remaining or Paid
off field — that becomes the new starting point and the automatic monthly steps continue from
there.

## Building a history over time

Since nothing is saved automatically on most pages, the easiest way to track net worth or
your mortgage paydown over time is to click **Export** periodically (monthly, say) on the
Net Worth or Home & Mortgage pages and keep the dated files. There's no built-in chart across
snapshots, but if you'd like one later, I can build a page that reads in a folder of these
export files and plots the trend.

## If you want to host it later

Whenever you're ready, this can go on GitHub Pages the same way as before: create a repo,
upload all six `.html` files (drag-and-drop via **Add file → Upload files**, no terminal
needed), then **Settings → Pages → Deploy from a branch → main → /(root)**. `new_house.html`
will keep working the same way there since its sync talks to Supabase directly, not to
wherever the file itself is hosted.

# finance-ledger

**[Open the live app →](https://shanemc92.github.io/finance-ledger/)** - no download, nothing to
install, just opens and works.

A single-file personal finance workbook that runs entirely in your browser.

For years I ran my household finances out of one Excel workbook: bills, an Irish PAYE calculator
I kept re-tuning every Budget, loan amortisation schedules, a bank statement categoriser, savings
projections, a historical bill log. It worked fine and never broke - the problem was everything
else around it. Making a change meant opening a laptop. Simulating something temporary, like what
maternity leave does to a few months of cash flow, meant duplicating sheets and hoping I remembered
to delete them. And there was no version I could pull up on my phone standing in a shop.

So I rebuilt it as a web app. One HTML file, no build step, no dependencies, no server. Open it
and it works, offline, on a phone or a laptop, and a "what if" scenario is a tab you fill in and
switch off again rather than a sheet you have to remember to clean up. Your data lives in that
browser's local storage and nowhere else.

![Dashboard](docs/screenshots/dashboard-desktop.png)

## What it does

**Budget** - bills at any frequency from weekly to annual, normalised to annual, monthly and
weekly figures. Savings and investments are tracked in a separate table from bills, so "what do I
spend" and "what do I put away" stay distinct, and anything can be moved between the two with one
click. Assign each line to a person or to the joint pot, and split the joint pot three ways:

- **Share of net income** - pro-rata on what each person takes home
- **Manual percentages** - whatever you decide, scaled to fit if they do not add to 100
- **Even weekly disposable income** - solved so everyone is left with the same amount each week
  after their own bills, their savings and their share of the joint pot

Each person's leftover is worked out after all three, weekly and monthly.

**Tax** - Irish PAYE, USC, PRSI and pension for as many earners as you need. Standard rate band,
credits and USC bands are all editable, with the published figures for the year preloaded. Handles
BIK, AVCs and a bonus taxed at the margin.

**Loans** - daily-interest amortisation, the same method my spreadsheet used. Model a higher
repayment or drop lump sums onto individual payments and see the interest saved and how many
payments earlier it clears. Set the date the overpayment started and it works out where the loan
actually stands today - what is still owed, and what interest is yet to be charged between now and
the final payment.

**Mortgage** - kept apart from Loans because a mortgage does not behave like one. Interest accrues
daily on the cleared balance and the repayment is not a figure you choose: the lender re-solves it
over whatever is left of the original term every time the rate changes. Put the whole rate schedule
in - the fix you drew down on, the follow-on rate dated the day it ends - and the table tells you
what the repayment becomes when you roll off. Type in the balance off each statement as it arrives -
once a year is plenty, which is all most lenders send - and the years of modelling in between are
adjusted onto it, so the schedule tracks the real account instead of drifting away from it.
Overpayments and lump sums shorten the term rather than
the payment, a lump comes off from its own date, and the whole thing is costed against the same
mortgage with the overpayments taken back out so "interest saved" means something. See
[Mortgage](#mortgage-notes) below.

**Bank** - import a CSV statement (AIB export format, or anything with date/description/debit
columns - headers are matched automatically). Rules map descriptions to categories, duplicates are
skipped on re-import, and everything rolls up against the budget with weekly and monthly averages.
Rules are re-applied automatically whenever a file is loaded, so a restored backup lands with its
spending already categorised.

**Accounts** - your bank accounts and your payees in one place: owner, bank, type, IBAN, BIC and a
note. IBANs are masked by default, validated with the standard mod-97 checksum so a transposed
digit shows up immediately, and copyable with one click. Payees use the same fields with a free
text owner, so it can be a person, a tradesman or a company.

**Savings** - a per-person breakdown of what is being put aside, with joint amounts split evenly
regardless of income, plus multiple accounts, contributions, optional growth and a projection out
as far as you like. Money taken back out gets logged against the month it happened, so the
projection stays honest about the dips.

**Electricity** - import the HDF file from ESB Networks and compare day/night/peak tariffs against
what you actually used. It takes the last twelve complete months, buckets every half-hourly read
into its rate band, and costs each offer you enter including standing charge, discount and VAT.
A year of readings is around 17,500 rows; only the per-band monthly totals and an average-day
profile are saved, so the file stays small. See [Electricity](#electricity-notes) below.

**Maternity leave** (optional, switch it on in Data) - model a period of leave against the budget.
Set the start date, paid and unpaid weeks, the Maternity Benefit rate, and as many employer pay
periods as the policy has (full pay for twelve weeks then half for fourteen, say). It works out
month by month what that person actually takes home, how far below normal that is, and what the
household is left with after bills and savings. A per-person table shows weekly disposable income
now against weekly disposable income during the leave, using the same bill split as the Budget tab,
and there is a toggle to stop their savings for the period to bridge the gap.

**Contracts** - end dates for anything with a fixed term: broadband, insurance, energy, mobile.
Anything inside 60 days shows on the dashboard, so it's in front of you whenever you open the app,
and "Renewed" rolls the date on a year once you've sorted it. There are deliberately no push
notifications: nothing here runs unless the page is open, so an alert could only ever fire at the
moment you're already looking at the dashboard that lists it. If you want reminders that arrive on
their own, put the renewal dates in a real calendar.

**History** - a log of what bills actually cost month by month, kept across years, so next year's
budget is set from real numbers instead of guesses.

| | |
|---|---|
| ![Budget](docs/screenshots/budget-desktop.png) | ![Loans](docs/screenshots/loans-desktop.png) |
| Bills and savings, split out, with the income split | Loan schedules and overpayments |
| ![Electricity](docs/screenshots/electricity-desktop.png) | ![Accounts](docs/screenshots/accounts-desktop.png) |
| Tariff comparison and the average-day profile | Accounts and payees |

<p align="center">
  <img src="docs/screenshots/dashboard-mobile.png" width="200" alt="Dashboard on mobile">
  <img src="docs/screenshots/electricity-mobile.png" width="200" alt="Electricity on mobile">
  <img src="docs/screenshots/loans-mobile.png" width="200" alt="Loans on mobile">
</p>

## Running it

Pick whichever of these fits how you want to use it - they all run the exact same page.

### 1. Use it straight from GitHub Pages (quickest)

**[Open the app →](https://shanemc92.github.io/finance-ledger/)**

No download, no account, nothing to install - it opens straight in your browser. On a phone, add
it to your home screen (share icon → *Add to Home Screen* on iPhone, browser menu → *Add to Home
screen* on Android) and it behaves like a normal app icon from then on. Nothing you type is sent
anywhere - the page is just being served from GitHub, and your data only ever lives in your own
browser's storage on your own device.

### 2. Download it and open it locally

1. Go to the [repo on GitHub](https://github.com/shanemc92/finance-ledger), click the green
   **Code** button near the top, then **Download ZIP**.
2. Unzip it - most phones and computers do this automatically when you tap or double-click the
   `.zip` file.
3. Open `index.html` from inside the unzipped folder. It opens in your normal browser and that's
   it - you're using it.

This works fully offline once downloaded, but it only exists on the device you saved it to, and
it's a slightly clunkier tap-through on a phone (open Files, find the folder, find the file) than
a bookmark.

### 3. Run your own copy, your own way

If you'd rather have an independent copy - to make your own changes, or just so it isn't tied to
someone else's repo - `index.html` is the entire application, so any of the usual ways to host a
static site work:

- **Your own GitHub Pages**: click **Fork** on the
  [repo page](https://github.com/shanemc92/finance-ledger) to make your own copy under your
  account, then in your fork go to **Settings → Pages**, set **Source** to **Deploy from a
  branch**, branch **main**, folder **/ (root)**, and **Save**. After a minute or two it's live at
  `https://<your-username>.github.io/finance-ledger/`.
- **Netlify, Vercel, or similar**: drag the folder in, or connect the repo - `index.html` at the
  root is all either one needs.
- **Your own web space or static server**: upload `index.html` and point your domain at it.

### 4. Self-host it with sync between machines

Everything above keeps your data in one browser on one device. If you have somewhere to run a
container, [`homelab/`](homelab/) is a small server that reads and writes a JSON file on your own
machine instead, so the same ledger follows you between devices with no exporting and importing.
It serves this same `index.html` - the sync code is already in it and stays dormant everywhere
else:

```bash
docker run -d --name finance-ledger -p 8080:8080 -v /srv/finance-ledger:/data ghcr.io/shanemc92/finance-ledger:latest
```

`amd64` and `arm64`, no login of its own - put it on your LAN or behind whatever auth you already
run. See [homelab/README.md](homelab/README.md) for the details and the security notes.

### A note on your data either way

However you run it, your entries stay on that one device, in that one browser. Switching phones,
clearing your browsing data, or opening the app in a different browser all start you with nothing
- so get in the habit of using **Data → Download all years (JSON)** every so often, and keep the
file somewhere safe (email it to yourself, save it to cloud storage). Restoring it later is the
same Data tab, in reverse.

## Try it with demo data

`demo/ledger-demo-2026.json` is a fictional two-income household: 401 transactions, a mortgage part
way through a fixed rate, three loans, four savings accounts, five bank accounts, four payees, a
year of smart meter usage and two years of bill history. Nothing in it is real.

1. Open `index.html`
2. Data tab -> **Load demo data**

It's built into the app, so there's nothing to download separately - the button loads it straight
into whichever year is currently open. If that year already has your own data, it asks first.
The same file also sits at `demo/ledger-demo-2026.json` if you'd rather restore it the normal way
(Data tab -> Restore from JSON).

## Your data

Everything is held in `localStorage` under a single key. It is never transmitted, and there is no
analytics, no telemetry and no fonts or scripts loaded from anywhere.

The one request the page makes is to its own address, once, on open: it asks whether it is being
served by a [self-hosted ledger server](homelab/) that wants to sync. That request carries none of
your data, and on GitHub Pages - or any other static host, or the file opened off a disk - nothing
answers it and the page carries on with `localStorage` alone, making no further requests.

That also means clearing site data wipes it, so take backups:

- **All years (JSON)** - full backup, restores everything
- **Single year (JSON)** - move one year between browsers or share a scenario
- **Workbook (.xlsx)** - a sheet per section (bills, savings commitments, savings split, tax, bank
  analysis, transactions, rules, savings projection, history, accounts, payees, electricity usage,
  offers and profile, plus one per loan and one per mortgage), written natively without a library
- **CSV** - per section, if you want to pull something into a spreadsheet

"+ Year" rolls bills, loans, the mortgage, savings, rules, people, accounts, payees, electricity
tariffs and the maternity plan into a new year, applies that year's tax figures, and starts
transactions and meter usage empty.

**Duplicate** takes a complete copy of the current year under a new name - "2026-2", say - so you
can put in changed figures halfway through a year, or keep a couple of what-if scenarios side by
side and flick between them in the year dropdown. Nothing is shared between the copies.

## Tax figures

Preloaded for 2024, 2025 and 2026 from the Budget and Revenue published rates. For 2026:

| | |
|---|---|
| Standard rate band | 44,000 single (up to 53,000 individual cap where bands are shared) |
| Rates | 20% / 40% |
| USC | 0.5% to 12,012, 2% to 28,700, 3% to 70,044, 8% above. Exempt under 13,000 |
| PRSI | 4.2%, rising to 4.35% from 1 October |
| Credits | 2,000 personal, 2,000 employee |

Every one of those is editable if your circumstances differ. To add a future year, create it and
either edit the bands or add an entry to `TAX_PRESETS` in the source.

This is an estimate for planning, not a payslip and not tax advice. It does not model married band
transfers automatically, week-1 basis, medical card USC rates, or anything self-employed. Check
anything that matters against Revenue.

## Maternity leave notes

Irish figures, all editable: 26 weeks paid leave plus up to 16 weeks additional unpaid, and
Maternity Benefit at 299 a week for 2026 (Budget 2026 added 10 to the 2025 rate).

Maternity Benefit is taxable but exempt from PRSI and USC - Revenue collects the tax by trimming
your credits and rate band, which normally works out at 20% while income is down. That is the
default and it is editable.

Employer pay periods run in sequence from the start of the leave, each with its own policy: full
pay topping up the benefit, a percentage of salary, or a flat monthly amount. Anything declared
past the end of the leave is ignored.

Employer pay is costed at the marginal rates for the **leave year** rather than a normal year,
because a long leave often drops someone out of the higher band entirely. A refund frequently
turns up after year end once a full year of credits has been spread across reduced income; this
does not try to predict that, so treat the shortfall as the cautious figure.

Pausing savings closes the monthly cash gap but does not change where the household ends up - the
money either goes into a savings account or stays in the current account to pay the bills. It
matters when the savings sit somewhere you would rather not touch.

## Mortgage notes

Irish conventions, and where they stop:

- **How the interest is worked out is a setting, because lenders genuinely differ.** Two options:
  *daily, 1/365th* accrues on the cleared balance every day and splits a period that straddles a
  rate change at the change date, which is what most lenders describe; *monthly, 1/12th* charges a
  flat twelfth of the annual rate at each payment and counts no days at all, which is what a good
  number of them actually do. On a 370k balance the two are about 20 a year apart. The stub between
  drawdown and the first payment is charged daily either way, which is universal.
- If every statement's interest comes out **the same amount off in the same direction**, that is
  the wrong setting rather than an error - switch it and watch the column. Tested against a real
  EBS mortgage, daily left roughly 22 a year unexplained and monthly tied out to the cent on all
  four annual statements.
- The repayment is solved with the monthly rate that matches the setting, so a schedule left alone
  lands on zero at the end of the term. Because a repayment is a whole number of cents it never
  divides a balance exactly, so the final payment takes up the few euro left over.
- **Statements are readings, not payments.** The monthly payments are always modelled; what a
  statement does is pin the balance. Everything modelled since the statement before it is adjusted
  onto the figure the lender printed, and the adjustment is shown so you can see how much drift it
  took out. One statement a year - which is all most lenders send - is enough to keep years of
  modelling honest, and an annual statement is *not* treated as a payment, which would otherwise
  count the year twice over. The interest and amount off the statement are optional and purely to
  check the model against: a few euro a year apart is day-count convention, hundreds means a wrong
  rate or date.
- When the rate changes the repayment is recalculated over the payments remaining out of the
  **original** term - the term does not stretch. Overpaying does the opposite: the repayment stays
  put and the term shortens, which is the Irish default unless you ask to re-amortise.
- Picking up mid-mortgage, put the repayment from your statement in the "repayment now" box. After
  a few overpayments the lender is still collecting the contractual amount, not one re-solved off
  the lower balance, and only your statement knows what that is. Leave it at 0 and it is worked out
  from the rate and the term.
- Fixed rates cap what you can overpay in a year before a break funding fee applies - commonly 10%,
  but it varies and it is in your loan offer, so the allowance is editable and going over only ever
  warns. **The fee itself is not estimated**: it depends on funding rates on the day, and guessing
  it would be worse than leaving it to you to ask.
- Insurance collected with the repayment - mortgage protection, home cover, whatever your lender
  bundles into the direct debit - goes in as one figure. It makes the amount leaving the account
  right without ever touching the balance, because it is not interest and not capital.
- Nothing on this tab feeds the Budget tab. Add the repayment there as a bill if you want it in the
  household budget.

Mortgages are deliberately left out of the dashboard's "loans still to pay" figure and given their
own, because a 300k mortgage swamps a car loan and makes that number useless for the thing it is
there for.

## Electricity notes

Get your HDF file from the ESB Networks customer portal - it is the half-hourly export for your
MPRN, in the format `MPRN, Meter Serial Number, Read Value, Read Type, Read Date and End Time`.

- Rate windows default to the common Irish ones (night 23:00-08:00, peak 17:00-19:00, day for
  everything else) and are editable, including windows that wrap past midnight.
- Readings are the **end** of each interval, so the row stamped 08:00 covers 07:30-08:00 and counts
  as night. Getting that backwards shifts half an hour of cheap units into the day band every day.
- kW and kWh read types are both handled, and the interval length is derived from the data rather
  than assumed.
- If you have less than twelve months, the figures are scaled to a full year.
- Only the summary is saved: monthly totals per band plus a 48-slot average day, about 1.4 KB from
  a 2 MB file.

The average-day chart colours each half hour by its rate band and shades the night and peak
windows, which makes it obvious whether a day/night/peak plan is worth switching to or whether a
flat 24-hour rate suits you better.

## Notes on the build

- One file, no framework, no CDN
- Charts are hand-drawn on canvas, sized from their container so they scale cleanly on retina
  displays and down to phone width
- XLSX export is a minimal store-only ZIP plus inline-string sheet XML, so there is no library to
  keep current
- Light and dark themes, keyboard focus states, works down to a 320px screen

## Licence

MIT - see [LICENSE](LICENSE).

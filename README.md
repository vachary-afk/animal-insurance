# Shory Animal Care — V2 prototype

A clickable prototype of the pets, horses and camels proposition discussed with
D1 Management. One self-contained HTML file: no build step, no dependencies, no
package install.

**This is a prototype, not a product.** Premiums are illustrative and no payment
is taken. See *Before this goes anywhere near a customer* at the bottom.

---

## What it does

Four steps: **Your animals → Your package → Your quote → Secure it**, then a
simulated payment and an issued policy with a printable schedule.

- **Three species.** Pets (dogs and cats), horses and camels, each with its own
  question set. Horses and camels carry an agreed value for the death benefit;
  pets do not, because pet mortality is a capped benefit.
- **Bronze / Silver / Gold / Platinum**, priced on annual limits rather than a
  fixed pot per treatment. Pets run to AED 100k, camels to AED 180k, horses to
  AED 250k.
- **Add-ons at cost.** Anything a package leaves out can be bought individually,
  so a customer on a low vet limit need not give up loss of use or tack cover.
- **D1 direct billing.** Choosing the D1 network is 10% cheaper and removes claim
  forms; any licensed vet is pay-and-claim at full rate.
- **Takaful by default**, with Conventional one tap away. Takaful switches the
  contract language throughout — contribution, participant, Takaful operator.
- **Referral routing.** Anything outside the online rules (age, value, an existing
  condition, racing or stud use) skips the price and routes to the team.

---

## Deploying to Vercel

Static site, no framework. Vercel will detect it as "Other" — that is correct.

### Option 1 — drag and drop (fastest)

1. Go to <https://vercel.com/new>
2. Drag this whole folder onto the page
3. Deploy. Done in about thirty seconds.

### Option 2 — Vercel CLI

```bash
npm i -g vercel
cd shory-animal-care-v2
vercel
```

Accept the defaults. Run `vercel --prod` when you want the production URL.

### Option 3 — GitHub

```bash
cd shory-animal-care-v2
git init && git add . && git commit -m "Shory Animal Care V2 prototype"
git branch -M main
git remote add origin git@github.com:<org>/<repo>.git
git push -u origin main
```

Then import the repo at <https://vercel.com/new>. Every push to `main` redeploys.

**Build settings, if it ever asks:** Framework `Other`, build command *empty*,
output directory `.`, install command *empty*.

---

## Configuration

Everything you are likely to change sits in one block near the top of the
`<script>` in `index.html`:

```js
const SUPABASE_URL   = ''                      // leave blank for the mailto fallback
const SUPABASE_ANON  = ''
const REFERRAL_EMAIL = 'corporate@shory.com'   // where quotes and referrals land
```

**While `SUPABASE_URL` is blank**, submitting opens the sender's own email client
with a fully populated message to `REFERRAL_EMAIL`. That genuinely works — but it
comes from the sender's address and they have to press send.

**To capture submissions server-side**, point `SUPABASE_URL` and `SUPABASE_ANON`
at a Supabase project with an Edge Function named `animal-care-quote`. The front
end POSTs the full JSON payload (animals, package, add-ons, network, contract
basis, premium, referral reasons, contact) and expects a 2xx.

### Pricing

All rates live in clearly marked constants and are safe to edit in place:

| Constant | Controls |
|---|---|
| `PACKAGES` | Tier limits, excess, and the base price per tier per species |
| `BENEFITS` | What sits at which tier, plus `addon` / `addonPct` and `cap` |
| `MORTALITY_RATES` | Death-benefit rate on agreed value |
| `PET_AGE`, `EQUINE_AGE` | Age loadings |
| `NETWORKS` | The D1 versus any-vet differential |
| `MAX_VALUE_ONLINE`, `MAX_ANIMALS`, `MIN_PREMIUM` | Online underwriting limits |

---

## Before this goes anywhere near a customer

1. **The premiums are invented.** They are proportioned sensibly against each
   other but carry no insurer rates, no claims data and no view of what the D1
   network costs. Treat the ladder as the shape of the proposition, not the
   numbers.
2. **Payment is simulated.** Tabby, Apple Pay and Card are visual only. Nothing
   is charged and no card data is captured or transmitted. The page says so in
   three places — remove those lines only when a real gateway is wired in.
3. **The policy schedule is marked as a prototype output** and states it is not a
   contract of insurance. Keep that line until the wording is signed off.
4. **No personal data is stored or sent anywhere** in the default configuration.
   The Emirates ID and date of birth are used in-page only. Once a backend is
   connected, that changes, and it will need a privacy review.
5. **The identity check is cosmetic.** The Emirates ID and date of birth are
   validated for format only. Real verification needs a UAE Pass or ICP
   integration.

---

## Browser support

Modern Chrome, Edge, Safari and Firefox. Light and dark themes both supported —
it follows the viewer's system setting. Responsive down to mobile.

Built on the Shory brand tokens from Foundry (`Variables › Shory` and `Global`),
with component geometry taken from the Foundry Button and Form Field specs.

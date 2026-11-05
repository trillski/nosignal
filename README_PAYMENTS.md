# NO SIGNAL payment integration and release gate

Updated: November 5, 2026.

## Current state

- Public test server: https://no-signal-1159.onrender.com
- Sales page with a 45-second free sample: https://no-signal-1159.onrender.com/offer
- Release candidate price: $14.99 USD per **group host**, allowing up to 5 invited nonpaying guests.
- Customer also gets an HTML offline file (`/download` after purchase) with offline 1–6-person gameplay.
- GitHub source is **public**, so it is not currently a secure commercial distribution of story/code. Owner should switch repo visibility to private in GitHub settings before claiming exclusivity; Render's GitHub connection must continue to have access.
- Stripe account is **not** connected/configured. Checkout is disabled and no purchases are possible. No sales/revenue claimed.

## Architecture

- `/offer` always shows the product landing page, original 45-second checksum mini-puzzle, and current payment status.
- When `NO_SIGNAL_SALES_MODE=preview` (current): `/` remains the original open playable beta; `/api/checkout` rejects purchases.
- Once a live merchant account and all release checks are ready, set `NO_SIGNAL_SALES_MODE=live` in Render: `/` becomes the storefront and `/play` is restricted to buyers or invited guests. The full game JS and data files cannot be fetched by unauthenticated browsers. `POST /api/create` is restricted to buyers.
- Successful payment is verified server-to-server against Stripe Checkout Session status, amount, product SKU and livemode. Server issues an HttpOnly, SameSite=Lax, Secure, HMAC-signed access cookie that survives Render restarts.
- Buyers can download a self-contained HTML playable offline copy. Guests join by a case-room invitation URL, need no purchase, and may not create their own paid rooms or download the file.
- Signed buyer access does not currently include a self-serve recover-my-purchase email flow. The success redirect is not a substitute for webhook-based fulfillment: add Stripe webhook handling, email notifications or a platform-managed download before accepting paid customers who might close the return page. See https://docs.stripe.com/payments/checkout/fulfillment.

## Required env vars

- `NO_SIGNAL_PUBLIC_URL=https://no-signal-1159.onrender.com` (configured)
- `NO_SIGNAL_ACCESS_SECRET`: generated, private, random long signing secret, currently configured in Render (never commit this secret)
- `STRIPE_SECRET_KEY`: LIVE restricted or secret API key stored **only in Render environment variables**, not source code or chat (NOT configured)
- `NO_SIGNAL_SALES_MODE=live` only after a verified live test checkout, merchant verification, refund/support information and fulfillment reliability checks. Currently `preview`.

## Sales and production cautions

- Payment processor fees and tax obligations exist even with $0 monthly software cost.
- The game stores all room data in RAM; active rooms are lost if the free Render service restarts/sleeps and cold start may delay access. The offline copy is permanent in the customer's device.
- Before real purchases: verify browser checkout redirect, receipt or recovery, 6-player real-device play, refund/support instructions, and that the data served to buyers matches the product description.
- Do not claim checkout is live until an actual successful test purchase is independently verified and refunded appropriately.
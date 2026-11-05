# NO SIGNAL — secure production credential handoff

**Status:** This document intentionally contains NO credentials. Render is running in `NO_SIGNAL_SALES_MODE=preview`, so visitors cannot be charged. Render service: `srv-dbmdeim1egvs73apub20`, dashboard: https://dashboard.render.com/web/srv-dbmdeim1egvs73apub20 .

## What has already been done
- Live Stripe account NoSignal is connected; payments and payouts enabled.
- Live product Case 01 is active at $14.99 USD.
- Live price ID is configured in Render under `NO_SIGNAL_PRICE_ID`.
- Live Stripe webhook has been created and enabled for Checkout events at `/api/stripe/webhook`.
- GitHub source includes hosted Checkout Session creation, paid-session verification, guarded game delivery, guest links, receipt recovery, signed webhook handler, and secret-free readiness status.
- Browser-facing checkout stays OFF on purpose.

## One-time credential handoff (owner enters values DIRECTLY in protected Render Dashboard)
Open **Render Dashboard → no-signal-1159 → Environment → Environment Variables**. Do not provide credentials to chat or commit them to GitHub.

- `STRIPE_SECRET_KEY`: Create/reveal a **LIVE** API key in https://dashboard.stripe.com/apikeys . A least-privilege `rk_live_` restricted key with Checkout Sessions Read/Write (and required Customer permissions) is preferred; a `sk_live_` server-side secret key also works, but gives broader account access.
- `STRIPE_WEBHOOK_SECRET`: Reveal the **LIVE** signing secret beginning `whsec_` for the endpoint `https://no-signal-1159.onrender.com/api/stripe/webhook` at https://dashboard.stripe.com/webhooks . Do not use the sandbox's signing secret.
- `NO_SIGNAL_ACCESS_SECRET`: Existing deployment documentation says an independent signing secret is already configured. If absent, generate a new random 32+-character value in a password manager and enter directly in Render. Do not copy it into chat.
- Leave `NO_SIGNAL_PRICE_ID` set to the existing live price and `NO_SIGNAL_SALES_MODE=preview`.

Select **Save and deploy** in Render. **Do not set live mode in the Dashboard yet**.

## Automated follow-up
Inspect public endpoint `/api/commerce/status` and its new `readiness` booleans. `liveStripeKeyPresent`, `livePriceConfigured`, `webhookSigningPresent`, `accessSigningPresent` should all be true. No secret values are returned.

Then: perform actual end-to-end Stripe test-mode simulated payment and customer recovery QA (not a real-charge claim), verify the live endpoint can create a session and that buyer delivery is reliable, review refund/support and source visibility, and only after verification switch `NO_SIGNAL_SALES_MODE=live` through Render. Real-money checkout is never to be switched on merely because booleans are true.

## Security constraints
Never echo secrets in logs, ChatGPT or GitHub; never transfer a secret via a blocked tool action. Avoid storing any card details. Render's free service can restart, deleting live rooms; the downloadable offline game remains usable. Source repository `trillski/nosignal` is public and should become private before marketing an exclusive paid game, while retaining Render's GitHub access.

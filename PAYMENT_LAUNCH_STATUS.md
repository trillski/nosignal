# NO SIGNAL: 11:59 — Payment launch status (Nov 5, 2026)

## Implemented
- Public six-player beta: https://no-signal-1159.onrender.com
- Sales page: https://no-signal-1159.onrender.com/offer
- One-time $14.99 per host, five nonpaying guest invites, offline download
- Stripe hosted Checkout Sessions with a registered price, fixed SKU and server-side session verification
- Signed webhook endpoint /api/stripe/webhook for completed/async succeeded payment events
- Signed HttpOnly access, guarded guest routes, purchase recovery link after successful verification
- Stripe sandbox product prod_VZgjHF5LWSCz4F, price price_1UYXM10LjGVamGJ1bwZ9c8RL, tax code txcd_10201000
- Stripe sandbox webhook we_1UYXPe0LjGVamGJ1Sro88sfv; secret stored in Render env (never here)
- Test Checkout Session created in Stripe sandbox: mode payment, $14.99, unpaid/open (no card charged)
- Local simulated gateway, webhooks, access and offline gameplay tests passed
- Render service srv-dbmdeim1egvs73apub20, successful deployment dep-dbmej2h5efls73e43t10

## Deliberate launch gate
NO_SIGNAL_SALES_MODE remains preview. Do not accept real charges until:
1. A verified live Stripe merchant account is available, along with its live key and webhook secret securely configured in Render.
2. A new live product/price at $14.99 is created; configure NO_SIGNAL_PRICE_ID to that **live** price ID, not the current sandbox price.
3. Webhook events and real browser purchase, recovery and download flows are end-to-end tested.
4. Public GitHub repository exposure, support/refund policy, customer email recovery and room volatility are reviewed.

Only Stripe sandbox is currently authorized via plugin. Never place API keys in GitHub or chat. Never treat a sandbox session as real revenue. This document is a status/handoff, not evidence that payment fulfillment or live commercial sales have been validated.

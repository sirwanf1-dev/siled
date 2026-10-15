# SILED checkout launch checklist

SILED is currently a static storefront preview. The basket is interactive, but checkout is deliberately disabled. Product prices shown on the site are examples and must not be used to take real orders.

## Recommended launch architecture

- Storefront: Cloudflare Pages connected to the public GitHub repository `sirwanf1-dev/siled`.
- Checkout: Stripe Checkout created by a server-side Cloudflare Pages Function.
- Payment methods: cards first; enable Google Pay through Stripe once the domain, HTTPS and payment configuration meet Stripe's requirements.
- Never put Stripe secret keys in `index.html`, other public files, or GitHub commits. Store secrets in Cloudflare Pages environment variables.

## Steps before accepting real payments

1. Create a Stripe account using the real business details and complete Stripe's verification requirements. Add the business bank account for payouts.
2. Confirm each product's real selling price, product code, stock, accurate product image, and whether the displayed price includes VAT. Replace all sample prices.
3. Set UK delivery rates and rules, including free-delivery thresholds if offered, dispatch times, restricted items and how delivery is calculated.
4. Prepare and publish the business contact details, delivery policy, returns/refunds policy, privacy notice, terms of sale and any required VAT information.
5. Move the store from GitHub Pages to a hosting setup suitable for commercial checkout. Connect Cloudflare Pages to this repository and configure a Pages Function for checkout.
6. Add Stripe test-mode keys only as Cloudflare environment variables. Build a server-side checkout endpoint that accepts product IDs and quantities, looks up prices from a trusted server-side catalogue, and creates a Stripe Checkout Session. Never trust prices sent by the browser.
7. Add a Stripe webhook endpoint. Verify Stripe webhook signatures and use confirmed payment events—not the browser redirect—to mark orders as paid.
8. Test in Stripe test mode: successful payment, declined payment, cancelled checkout, duplicate webhook delivery, basket quantity changes, delivery fee, VAT, mobile layout, and order confirmation emails.
9. Before launch, check that orders are recorded securely, stock and delivery are correct, refunds work, and customer data is handled appropriately.
10. Only after all checks pass, configure live Stripe keys in Cloudflare environment variables and deliberately enable the live checkout button.

## Current safety status

- Basket preview: enabled.
- Displayed product prices: sample values; not verified.
- Checkout button: disabled.
- Card payments / Google Pay: not connected.
- Orders / stock management / emails: not connected.
- No payment credentials are stored in this repository.

## Important

Do not paste Stripe secret keys into chat or commit them to GitHub. Use the Cloudflare dashboard's encrypted environment-variable settings. The checkout endpoint must calculate prices on the server and must only fulfil an order after verifying payment with Stripe.

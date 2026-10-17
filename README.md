# Pressure Washing — Instant Estimate Page

A single-page quote calculator a pressure washing company can use to give a customer a price on the spot. The customer picks what needs cleaning, enters measurements, and the price updates as they type. On submit, the details and the quote are emailed to the office.

Live page: https://raynoralejo-collab.github.io/pressure-washing-estimate/

---

## Pricing

| Service | Rate | Measured in |
|---|---|---|
| House exterior | $0.15 | per square foot |
| Driveway | $0.12 | per square foot |
| Deck or patio | $0.30 | per square foot |
| Fence | $1.00 | per **linear** foot |
| Minimum job | $150.00 | applies to any job |

The minimum works like a floor, not a surcharge: if the line items come to less than $150, the customer pays $150. If they come to more, the minimum never applies. The estimate panel shows the subtotal and the adjustment separately so the customer can see exactly why the number is what it is.

**Worked example** — 1,800 sq ft house + 700 sq ft driveway + 120 linear ft fence:

```
House     1,800 sq ft     x 0.15 = $270.00
Driveway    700 sq ft     x 0.12 =  $84.00
Fence       120 linear ft x 1.00 = $120.00
                           Total = $474.00
```

**Minimum example** — 400 sq ft driveway only:

```
Driveway    400 sq ft     x 0.12 =  $48.00
                        Subtotal =  $48.00
       Minimum job adjustment    = +$102.00
                           Total = $150.00
```

---

## How the email works

The page posts to [FormSubmit](https://formsubmit.co) — a free form-to-email relay, no backend or server required, which keeps the site deployable as plain static files on GitHub Pages.

- The form endpoint is the address that owns the form.
- `careers@finchharbor.com` is set as a CC on every submission, so the office receives the customer's details and the quote on each send.
- The customer's email address is set as `Reply-To`, so replying to the notification goes straight back to the customer.
- A hidden honeypot field (`_honey`) filters out bot submissions.

### Setup

Open `index.html` and change one line near the top of the `<script>` block:

```js
var FORM_ENDPOINT = "https://formsubmit.co/ajax/YOUR_EMAIL_HERE";
var OFFICE_EMAIL  = "careers@finchharbor.com";
```

Submit the live form once. FormSubmit sends a one-time activation email to the endpoint address — click the link in it, and every submission from then on is delivered.

---

## Deploying

The site is one static file with no build step and no dependencies.

1. Create a public GitHub repository.
2. Upload `index.html` and `README.md`.
3. **Settings → Pages → Source: Deploy from a branch → `main` / `(root)` → Save.**
4. The page goes live at `https://<username>.github.io/<repo>/` within a minute or two.

---

## Interface

Three things the page does that a plain quote form doesn't:

**Before / after reveal.** A draggable comparison of dirty and clean siding sits above the form. The dirt is generated procedurally with an SVG turbulence filter, so there are no image files to host and nothing extra to load. It sweeps open once on page load to show it's interactive, and respects `prefers-reduced-motion`.

**Measurement helper.** Most customers don't know their square footage, which is where a quote form loses them. Each service has a "Not sure? Work it out" calculator — length × width for areas, runs × length for fence — that fills the measurement field automatically. Fewer abandoned quotes, and more accurate numbers coming into the office.

**Running total on mobile.** Once there's a price, a bar docks to the bottom of the screen with the live total and a button that jumps to the contact fields. The customer never scrolls away from the number they came for.

## Notes and assumptions

- **Fence is measured in linear feet**, not square feet — the input is labelled accordingly so customers don't measure the wrong thing.
- Each line item is rounded to the cent before being added, so the printed line items always add up to the printed subtotal.
- Blank, negative, and non-numeric measurements are treated as zero rather than throwing an error.
- The submit button is blocked until at least one service has a measurement and all contact fields are filled.
- Every quote gets a reference number (e.g. `CW-20261017-4821`) shown to the customer and included in the email, so a phone call can be matched to a submission.
- Copy describes this as an estimate based on customer-supplied measurements, with the final price confirmed on site — standard practice, and it protects the company when a customer's measurements are off.
- The company name, phone number, and branding are placeholders and can be changed in `index.html`.

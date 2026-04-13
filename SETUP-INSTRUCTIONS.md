# PRBE Purchase Page — Setup Instructions

This guide walks you through everything you need to get your PRBE purchase page live with Stripe payment processing.

---

## Overview

Your purchase system has three parts:

1. **Purchase Page** (`index.html`) — Collects customer info and redirects to Stripe
2. **Success Page** (`success.html`) — Shown after successful payment
3. **Stripe Payment Link** — Handles the actual payment (hosted by Stripe)

The pages are designed to be hosted on **GitHub Pages** (free) and linked from your existing **Google Sites** page.

---

## Step 1: Create a Stripe Account

1. Go to [https://stripe.com](https://stripe.com) and click **Start now**
2. Enter your email, full name, and create a password
3. Complete the business verification:
   - Select **Individual / Sole Proprietor** (or your business type)
   - Enter your personal and banking information (where payments will be deposited)
   - Stripe will verify your identity — this may take 1-2 business days

> **Important:** You can start building and testing in **Test Mode** immediately while verification completes.

---

## Step 2: Create a Stripe Payment Link

This is the simplest approach — no backend server needed.

1. Log in to your [Stripe Dashboard](https://dashboard.stripe.com)
2. In the left sidebar, click **Payment Links**
3. Click **+ New** to create a new payment link
4. Configure the product:
   - **Name:** PRBE Full License
   - **Price:** $29.99 (one-time)
   - **Description:** Lifetime license for PRBE — Probabilistic Risk-Based Estimating Tool
5. Under **After payment**, set:
   - **Confirmation page:** Custom URL
   - **URL:** `https://michaelsmith935-ux.github.io/prbe-purchase/success.html`
     *(update this to match your actual GitHub Pages URL — see Step 3)*
6. Click **Advanced options** and add a **Custom field**:
   - **Label:** Machine ID
   - **Type:** Text
   - **Required:** Yes
7. Under **Collect customer info**, enable:
   - Email (will be pre-filled from your page)
   - Full name
8. Click **Create link**
9. **Copy the Payment Link URL** — it will look like: `https://buy.stripe.com/live_xxxxxxxxx`

---

## Step 3: Update the Purchase Page

Open `index.html` in a text editor and find this line near the bottom:

```javascript
const STRIPE_PAYMENT_LINK = 'YOUR_STRIPE_PAYMENT_LINK_URL';
```

Replace `YOUR_STRIPE_PAYMENT_LINK_URL` with the Stripe Payment Link you copied in Step 2. For example:

```javascript
const STRIPE_PAYMENT_LINK = 'https://buy.stripe.com/live_abc123xyz';
```

Save the file.

---

## Step 4: Host on GitHub Pages

You have two options:

### Option A: New Repository (Recommended)

1. Go to GitHub and create a **new repository** named `prbe-purchase`
   - Make it **Public** (required for free GitHub Pages)
2. Upload both `index.html` and `success.html` to the repository
3. Go to **Settings** → **Pages**
4. Under **Source**, select **Deploy from a branch**
5. Select the `main` branch and `/ (root)` folder
6. Click **Save**
7. Your site will be live at: `https://michaelsmith935-ux.github.io/prbe-purchase/`

### Option B: Add to Existing PRBE Repository

1. Create a `docs` folder in your PRBE repo
2. Put `index.html` and `success.html` in the `docs` folder
3. Go to **Settings** → **Pages**
4. Set source to `main` branch, `/docs` folder
5. Your purchase page will be at: `https://michaelsmith935-ux.github.io/PRBE/`

---

## Step 5: Link from Google Sites

1. Go to [Google Sites](https://sites.google.com) and edit your PRBE site
2. You can either:
   - **Add a button** that links to your GitHub Pages purchase URL
   - **Add a new page** called "Purchase" with an embedded link
   - **Use the existing navigation** to add a "Buy Now" menu item

### Adding a Button on Google Sites:

1. Click **Insert** → **Button**
2. Set the button text to: **Purchase License — $29.99**
3. Set the link to: `https://michaelsmith935-ux.github.io/prbe-purchase/`
4. Style and position the button where you'd like it on your page
5. Click **Publish** to make the changes live

---

## Step 6: Set Up Email Notifications

When a customer pays, you need to know their email and Machine ID so you can send them a license.

### In Stripe Dashboard:

1. Go to **Settings** → **Email notifications**
2. Enable **Successful payments** notifications
3. This will email you every time someone purchases

### Viewing Customer Details:

1. Go to **Payments** in the Stripe Dashboard
2. Click on any payment to see:
   - Customer email
   - Customer name
   - **Machine ID** (under Custom Fields)
   - `client_reference_id` (also contains the Machine ID as backup)

---

## Step 7: Test Everything

### Test Mode (before going live):

1. In Stripe Dashboard, toggle to **Test Mode** (top-right switch)
2. Create a test Payment Link using the same steps as Step 2
3. Update `index.html` with the **test** Payment Link URL
4. Use Stripe's test card: `4242 4242 4242 4242` (any future date, any CVC)
5. Verify the full flow works:
   - Fill in email, Machine ID, and name on your page
   - Complete payment with the test card
   - Confirm you're redirected to the success page
   - Check Stripe Dashboard for the payment with Machine ID

### Go Live:

1. Switch back to **Live Mode** in Stripe
2. Replace the test Payment Link URL in `index.html` with the live one
3. Push the updated file to GitHub
4. Do one real test purchase (you can refund it afterward)

---

## How the Customer Flow Works

```
Customer visits your Google Sites page
        ↓
Clicks "Purchase License" button
        ↓
Arrives at GitHub Pages purchase page (index.html)
        ↓
Enters email, Machine ID, and name
        ↓
Clicks "Purchase License — $29.99"
        ↓
Redirected to Stripe Payment Link checkout
    (email pre-filled, Machine ID in custom field)
        ↓
Completes payment with credit card
        ↓
Redirected to success page (success.html)
        ↓
You receive Stripe notification with payment details
        ↓
You email the customer their license key
```

---

## Customization Notes

- **Price change:** Update the price in Stripe Payment Link AND in `index.html` (search for "29.99")
- **Colors:** Edit the CSS variables at the top of `index.html` under `:root`
- **Features list:** Edit the `<ul class="features-list">` section in `index.html`
- **FAQ:** Edit the FAQ section at the bottom of `index.html`
- **Success page URL:** Make sure the redirect URL in your Stripe Payment Link matches where `success.html` is actually hosted

---

## Troubleshooting

| Issue | Solution |
|-------|----------|
| Payment link not working | Make sure you replaced `YOUR_STRIPE_PAYMENT_LINK_URL` with your actual Stripe URL |
| GitHub Pages not loading | Ensure the repo is **Public** and Pages is enabled in Settings |
| Not receiving payment notifications | Check Stripe Dashboard → Settings → Email notifications |
| Customer's Machine ID missing | Verify the Custom Field is set to **Required** in your Stripe Payment Link |

---

## Support

- **Stripe Help:** [https://support.stripe.com](https://support.stripe.com)
- **GitHub Pages Docs:** [https://docs.github.com/en/pages](https://docs.github.com/en/pages)
- **Google Sites Help:** [https://support.google.com/sites](https://support.google.com/sites)

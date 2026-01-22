# 🚀 Stripe Integration Setup Guide

## Overview
Your QuickShop marketplace now has Stripe.js integrated for client-side checkout. For production, you'll need a backend server to handle payment processing securely.

## ✅ What's Already Done
- ✅ Stripe.js script loaded
- ✅ Checkout function integrated with cart
- ✅ User authentication check before checkout
- ✅ Currency support (USD, GBP, EUR)

## 🔧 Backend Setup Required

### Step 1: Create Stripe Account
1. Go to [stripe.com](https://stripe.com) and create account
2. Get your **Publishable Key** and **Secret Key** from dashboard

### Step 2: Set Up Backend Server

#### Option A: Node.js/Express (Recommended)
```bash
mkdir quickshop-backend
cd quickshop-backend
npm init -y
npm install stripe express cors dotenv
```

**server.js:**
```javascript
require('dotenv').config();
const express = require('express');
const cors = require('cors');
const stripe = require('stripe')(process.env.STRIPE_SECRET_KEY);

const app = express();
app.use(cors());
app.use(express.json());

app.post('/api/create-checkout-session', async (req, res) => {
  try {
    const { items, userId, userEmail, total, currency } = req.body;

    // Convert items to Stripe format
    const lineItems = items.map(item => ({
      price_data: {
        currency: currency,
        product_data: {
          name: item.name,
        },
        unit_amount: Math.round(item.price * 100), // Convert to cents
      },
      quantity: item.quantity,
    }));

    const session = await stripe.checkout.sessions.create({
      payment_method_types: ['card'],
      line_items: lineItems,
      mode: 'payment',
      success_url: `${process.env.FRONTEND_URL}/success?session_id={CHECKOUT_SESSION_ID}`,
      cancel_url: `${process.env.FRONTEND_URL}/cancel`,
      customer_email: userEmail,
      metadata: {
        userId: userId
      }
    });

    res.json({ id: session.id });
  } catch (error) {
    res.status(500).json({ error: error.message });
  }
});

app.listen(3001, () => console.log('Server running on port 3001'));
```

**.env file:**
```
STRIPE_SECRET_KEY=sk_test_your_secret_key_here
FRONTEND_URL=http://localhost:3000
```

#### Option B: Firebase Cloud Functions
```javascript
const functions = require('firebase-functions');
const stripe = require('stripe')(functions.config().stripe.secret);

exports.createCheckoutSession = functions.https.onCall(async (data, context) => {
  // Same logic as Express server
  const { items, userId, userEmail, total, currency } = data;

  const lineItems = items.map(item => ({
    price_data: {
      currency: currency,
      product_data: { name: item.name },
      unit_amount: Math.round(item.price * 100),
    },
    quantity: item.quantity,
  }));

  const session = await stripe.checkout.sessions.create({
    payment_method_types: ['card'],
    line_items: lineItems,
    mode: 'payment',
    success_url: `https://yourdomain.com/success`,
    cancel_url: `https://yourdomain.com/cancel`,
    customer_email: userEmail,
  });

  return { sessionId: session.id };
});
```

### Step 3: Update Frontend
Replace the test key in your HTML:
```javascript
const stripe = Stripe('pk_live_YOUR_ACTUAL_PUBLISHABLE_KEY');
```

### Step 4: Update Checkout Function
Replace the simulation with real API call:
```javascript
async function initiateCheckout() {
  if (!currentUser) {
    alert('Please sign in to checkout');
    return;
  }

  if (cart.length === 0) {
    alert('Your cart is empty');
    return;
  }

  try {
    const cartModal = bootstrap.Modal.getInstance(document.getElementById('cartModal'));
    cartModal.hide();

    showToast('Preparing checkout...');

    // Calculate total and prepare data
    let total = 0;
    const items = cart.map(item => {
      total += item.localPrice * (item.quantity || 1);
      return {
        name: item.name,
        price: item.localPrice,
        quantity: item.quantity || 1
      };
    });

    const checkoutData = {
      items,
      userId: currentUser.uid,
      userEmail: currentUser.email,
      total,
      currency: currencySymbol === '£' ? 'gbp' : currencySymbol === '€' ? 'eur' : 'usd'
    };

    // For Firebase Functions
    const createCheckoutSession = firebase.functions().httpsCallable('createCheckoutSession');
    const result = await createCheckoutSession(checkoutData);

    // Redirect to Stripe Checkout
    const { error } = await stripe.redirectToCheckout({
      sessionId: result.data.sessionId
    });

    if (error) {
      console.error('Stripe redirect error:', error);
      showToast('Checkout failed. Please try again.');
    }

  } catch (error) {
    console.error('Checkout error:', error);
    showToast('Checkout failed. Please try again.');
  }
}
```

## 🔐 Security Notes

- ✅ **Never expose secret keys** in frontend code
- ✅ **Use HTTPS** in production
- ✅ **Validate payments** on your backend
- ✅ **Store order data** in your database after successful payment

## 🎯 Testing Payments

### Test Card Numbers:
- **Success**: `4242 4242 4242 4242`
- **Decline**: `4000 0000 0000 0002`
- **Require auth**: `4000 0025 0000 3155`

### Webhook Setup (Production):
```javascript
app.post('/webhook', express.raw({type: 'application/json'}), (req, res) => {
  const sig = req.headers['stripe-signature'];
  const endpointSecret = process.env.STRIPE_WEBHOOK_SECRET;

  try {
    const event = stripe.webhooks.constructEvent(req.body, sig, endpointSecret);

    if (event.type === 'checkout.session.completed') {
      const session = event.data.object;
      // Fulfill the order
      console.log('Payment successful:', session.id);
    }

    res.json({received: true});
  } catch (error) {
    res.status(400).send(`Webhook Error: ${error.message}`);
  }
});
```

## 🚀 Deployment Checklist

- [ ] Stripe account created
- [ ] Backend server deployed
- [ ] Environment variables set
- [ ] Publishable key updated in frontend
- [ ] Webhooks configured
- [ ] SSL certificate installed
- [ ] Test payments working

Your marketplace now supports real payments! 🎉
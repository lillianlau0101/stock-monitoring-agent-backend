# 🚀 Stock Monitoring Agent

> [!TIP]
> This agent follows a **Fetch → Filter → Synthesize → Deliver** pipeline.

### 📂 Directory Structure
```text
stock-agent-backend/
├── src/
│   ├── agents/
│   │   └── researchAgent.js    <-- Gemini 3.1 Pro Logic
│   ├── services/
│   │   ├── newsService.js      <-- API calls to News/Earnings
│   │   └── mailService.js      <-- Sending the newsletter
│   ├── routes/
│   │   └── subscription.js     <-- Stripe $1/$5 logic
│   ├── templates/
│   │   └── newsletter.html     <-- The UI/UX for the email
│   └── app.js                  <-- Entry point
├── .env                        <-- API Keys (Gemini, Stripe, etc.)
├── package.json
└── README.md


Logic Breakdown (Visual View)
Diff
+ GREEN: Successful Data Fetching (Polygon.io / NewsAPI)
- RED: High-Risk Market Signals (Analyzed by Gemini)
! ORANGE: Upcoming Earnings Catalysts
@@ PURPLE: Personalized Delivery (Resend API) @@

import { GoogleGenerativeAI } from "@google/generative-ai";

const genAI = new GoogleGenerativeAI(process.env.GEMINI_API_KEY);

export const analyzeStockData = async (ticker, data) => {
  const model = genAI.getGenerativeModel({ model: "gemini-3.1-pro" });
  
  const prompt = `Analyze: ${JSON.stringify(data)}. Focus on the 'Decoded' brand voice.`;
  const result = await model.generateContent(prompt);
  return result.response.text();
};

import Stripe from 'stripe';
const stripe = new Stripe(process.env.STRIPE_SECRET_KEY);

// Marketing Logic: $1 Trial for 30 days, then $5/month recurring
export const createTrialSession = async (email) => {
  return await stripe.checkout.sessions.create({
    success_url: '[https://decoded.fm/success](https://decoded.fm/success)',
    line_items: [{ price: 'PRICE_ID_FOR_5_DOLLARS', quantity: 1 }],
    mode: 'subscription',
    subscription_data: { trial_period_days: 30 },
  });
};

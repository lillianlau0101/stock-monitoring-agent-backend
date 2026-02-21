# stock-monitoring-agent-backend

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

Gemini said
To build a personalized AI agent that mirrors the logic of a high-end service like the one you saw, you need a modular backend. Below is a clean, production-ready repository structure using Node.js (JavaScript/TypeScript).

This setup uses a "Controller-Service" pattern, which is the industry standard for keeping your AI logic separate from your data fetching.


1. The "Brain": src/agents/researchAgent.js
This file handles the communication with Gemini. It takes raw news and turns it into a "personalized" summary.

JavaScript
import { GoogleGenerativeAI } from "@google/generative-ai";

const genAI = new GoogleGenerativeAI(process.env.GEMINI_API_KEY);

export const generatePersonalizedReport = async (userData, rawNews) => {
  const model = genAI.getGenerativeModel({ model: "gemini-3.1-pro" });

  const prompt = `
    You are a private equity analyst. 
    User Portfolio: ${userData.watchlist.join(", ")}
    Raw Data: ${JSON.stringify(rawNews)}
    
    Task: Create a 'Signal-to-Noise' report. 
    1. Filter out general market fluff.
    2. Focus only on events impacting the user's specific stocks.
    3. Format the output in clean HTML for an email newsletter.
    4. Include a 'Key Takeaway' section for each ticker.
  `;

  const result = await model.generateContent(prompt);
  return result.response.text();
};
2. The "Senses": src/services/newsService.js
You need to gather data before the AI can analyze it. This script fetches the latest earnings and news.

JavaScript
import axios from 'axios';

export const getMarketData = async (tickers) => {
  // Replace with actual API endpoints like Polygon.io or AlphaVantage
  const API_KEY = process.env.MARKET_DATA_KEY;
  
  const newsPromises = tickers.map(ticker => 
    axios.get(`https://api.marketdata.com/v1/news/${ticker}?apikey=${API_KEY}`)
  );

  const results = await Promise.all(newsPromises);
  return results.map(res => res.data);
};
3. The Orchestrator: src/app.js
This ties everything together. It "wakes up," gets data, asks the AI to think, and sends the result.

JavaScript
import { getMarketData } from './services/newsService.js';
import { generatePersonalizedReport } from './agents/researchAgent.js';
import { sendEmail } from './services/mailService.js';

const runDailyAgent = async (user) => {
  try {
    console.log(`Starting run for ${user.email}...`);
    
    // 1. Fetch
    const rawData = await getMarketData(user.watchlist);
    
    // 2. Analyze (Gemini 3.1 Pro)
    const formattedNewsletter = await generatePersonalizedReport(user, rawData);
    
    // 3. Deliver
    await sendEmail(user.email, "Your Daily Decoded Insights", formattedNewsletter);
    
    console.log("Newsletter sent successfully!");
  } catch (error) {
    console.error("Agent failed:", error);
  }
};
AI agent architecture diagram, AI generated

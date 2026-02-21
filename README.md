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

<div align="center">

<img src="https://readme-typing-svg.demolab.com?font=Nunito&weight=700&size=28&pause=1000&color=E8A5B0&center=true&vCenter=true&width=500&lines=%F0%9F%93%A1+DeepSignal;Your+gentle+stock+intelligence+companion+%F0%9F%8C%B8" alt="DeepSignal" />

<br/>

**不再被市场噪音淹没 · 只看真正属于你的信号** 🌿

<br/>

![Node.js](https://img.shields.io/badge/Node.js-18+-FFB7C5?style=for-the-badge&logo=node.js&logoColor=white)
![Gemini](https://img.shields.io/badge/AI-Gemini%201.5%20Pro-FFDDD2?style=for-the-badge&logo=google&logoColor=white)
![Stripe](https://img.shields.io/badge/Billing-Stripe-FFE5EC?style=for-the-badge&logo=stripe&logoColor=white)
![License](https://img.shields.io/badge/License-MIT-FFF0F3?style=for-the-badge)

<br/>

*[English](#-what-is-deepsignal) · [中文](#-项目介绍)*

</div>

---

<div align="center">

```
 🌸  fetch  →  filter  →  synthesize  →  deliver  🌸
```

</div>

---

## 🌷 What is DeepSignal?

Most investors drown in the same market noise every morning.
DeepSignal is here to be your calm, focused research companion.

> ✨ You give it your watchlist.
> It gives back a **beautiful, personalized newsletter** — filtered by AI, written for *your* portfolio, delivered to your inbox every morning.

No noise. No generic summaries. Just the signals that actually matter to you. 🍵

---

## 🌸 项目介绍

每天早上，大多数投资者都被同质化的市场资讯淹没。
DeepSignal 想做你安静、专注的研究伴侣。

> ✨ 你告诉它你的自选股清单
> 它返回一份**精美的个性化简报** —— 经过 AI 筛选，专为你的持仓而写，每天早上送进你的收件箱

零噪音。不是通用摘要。只有真正与你相关的信号。🍵

---

## 🗺️ How It Works / 工作流程

```
╭─────────────────────────────────────────────────────╮
│                                                     │
│   📋 Your Watchlist                                 │
│         ↓                                           │
│   🎛️  Controller  ──────────────────────┐           │
│         ↓                               ↓           │
│   📰 News Service              💳 Stripe Billing    │
│         ↓                                           │
│   🧠 Research Agent                                 │
│         ↓                                           │
│   ✨ Gemini 1.5 Pro                                 │
│         ↓                                           │
│   💌 Beautiful HTML Newsletter                      │
│         ↓                                           │
│   📬 Your Inbox                                     │
│                                                     │
╰─────────────────────────────────────────────────────╯
```

---

## 🌼 Core Modules / 核心模块

### 🧠 The Brain — AI Research Agent
`src/agents/researchAgent.js`

像一位私募分析师一样思考——但只关注你的持仓，过滤掉其他所有噪音。

```javascript
import { GoogleGenerativeAI } from "@google/generative-ai";

const genAI = new GoogleGenerativeAI(process.env.GEMINI_API_KEY);

export const generatePersonalizedReport = async (userData, rawNews) => {
  const model = genAI.getGenerativeModel({ model: "gemini-1.5-pro" });

  const prompt = `
    You are a private equity analyst.
    User Portfolio: ${userData.watchlist.join(", ")}
    Raw Data: ${JSON.stringify(rawNews)}

    Task:
    1. Remove general market noise.
    2. Focus ONLY on events impacting the user's holdings.
    3. Generate clean HTML formatted for an email newsletter.
    4. Include a "Key Takeaway" section per ticker.
  `;

  const result = await model.generateContent(prompt);
  return result.response.text();
};
```

---

### 👀 The Senses — Market Data Layer
`src/services/newsService.js`

实时抓取你关注股票的新闻与收益数据，支持多数据源灵活切换。

```javascript
import axios from 'axios';

export const getMarketData = async (tickers) => {
  const newsPromises = tickers.map(ticker =>
    axios.get(`https://api.polygon.io/v2/reference/news?ticker=${ticker}&apiKey=${process.env.MARKET_DATA_KEY}`)
  );
  const results = await Promise.all(newsPromises);
  return results.map(res => res.data);
};
```

支持接入 · `Polygon.io` · `AlphaVantage` · `Finnhub` · `Bloomberg`

---

### 🎛️ The Orchestrator — Daily Runner
`src/app.js`

每天自动跑完整流程：拿数据 → 生成洞察 → 发邮件，安静又可靠。

```javascript
const runDailyAgent = async (user) => {
  const rawData    = await getMarketData(user.watchlist);
  const newsletter = await generatePersonalizedReport(user, rawData);
  await sendEmail(user.email, "✨ Your Daily DeepSignal", newsletter);
};
```

---

## 💳 Pricing / 订阅定价

<div align="center">

| 🌸 阶段 | 💰 价格 | 说明 |
|:-------:|:-------:|:----:|
| 甜蜜试用期 | **$1 · 30天** | 新用户专属引流价 |
| 正式订阅 | **$5 / 月** | 到期自动续费 |

</div>

Stripe 自动处理档位切换，无需任何手动操作 ✨

---

## 💌 Email Delivery / 邮件投递

- 📮 **发送服务：** Resend API
- 🎨 **模板：** 自定义 HTML，移动端响应式
- 📐 **排版风格：** 机构级简报 · 优雅易读

---

## 📂 Project Structure / 项目结构

```
deepsignalbeta/
│
├── 🧠 src/
│   ├── agents/
│   │   └── researchAgent.js      ← AI 推理核心
│   ├── services/
│   │   ├── newsService.js        ← 市场数据抓取
│   │   └── mailService.js        ← 邮件发送
│   ├── routes/
│   │   └── subscription.js       ← Stripe 订阅路由
│   ├── templates/
│   │   └── newsletter.html       ← 邮件 HTML 模板
│   └── app.js                    ← 主调度器
│
├── 🔐 .env.example
├── 📦 package.json
└── 📖 README.md
```

---

## 🔐 Environment Variables / 环境变量

```bash
cp .env.example .env
```

```env
GEMINI_API_KEY=your_google_gemini_key       # 🧠 AI 推理
MARKET_DATA_KEY=your_polygon_key            # 📰 市场数据
RESEND_API_KEY=your_resend_key              # 💌 邮件发送
STRIPE_SECRET_KEY=your_stripe_secret_key    # 💳 订阅计费
```

> 🌸 小提示：永远不要把 `.env` 提交到 GitHub，记得加进 `.gitignore` 哦

---

## 🚀 Quick Start / 快速开始

```bash
# 🌱 克隆项目
git clone https://github.com/lillianlau0101/deepsignalbeta.git
cd deepsignalbeta

# 📦 安装依赖
npm install @google/generative-ai axios express dotenv resend stripe

# 🔐 配置环境变量
cp .env.example .env

# ✨ 启动
node src/app.js
```

**部署选项：** Vercel · Railway · Render · AWS Lambda · DigitalOcean

---

## 🗺️ Roadmap

```
✅ AI 个性化简报生成
✅ 多数据源市场数据接入
✅ Stripe 订阅计费
✅ Resend 邮件投递
⬜ JWT 用户认证
⬜ PostgreSQL 持仓数据存储
⬜ Dashboard 分析面板
⬜ 风险评分模型
⬜ AI 驱动的交易预警
⬜ 多 Agent 并行架构
⬜ 用量计费（Usage-based billing）
```

---

## 🌿 Contributing

欢迎一起把 DeepSignal 做得更好 🌸
特别欢迎贡献：新数据源适配器 · 更好的 prompt 模板 · 邮件模板设计

1. Fork 这个项目
2. 新建你的 feature 分支 `git checkout -b feature/your-idea`
3. 提交改动 `git commit -m 'Add: your idea'`
4. 推送 `git push origin feature/your-idea`
5. 开一个 Pull Request 💌

---

## 📄 License

MIT © [Lillian Lau](https://github.com/lillianlau0101)

---

<div align="center">

🌸 · 🌿 · 🌸

**Less noise. More signal.**
*过滤噪音，只留真正属于你的信号。*

<br/>

⭐ 如果 DeepSignal 对你有一点点帮助，欢迎点个 Star～
它对独立开发者来说意味着很多 🌷

</div>

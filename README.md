# AI Marketing Platform

**AI-powered marketing automation platform** - Generate and publish ads, social posts, and emails across all channels from a single product URL.

> **Positioning:** Your entire marketing department, automated. $149/month vs. $15K/month team.

---

## 🎯 Vision

Combine the best of **Crush.ai** (Facebook ads automation) + **Holo.ai** (multi-channel content) with key differentiators:

| Crush.ai | Holo.ai | **Us** |
|----------|---------|--------|
| Facebook ads only | Multi-channel content | **Multi-channel + publishing** |
| ✅ Auto-publish | ❌ Generate only | ✅ **Auto-publish everywhere** |
| ✅ Auto-optimize | ❌ No optimization | ✅ **Cross-channel optimization** |
| $99 flat | Credit-based | ✅ **Tiered: $49/$149/$299** |

---

## 🚀 Features (Planned)

### Phase 1: MVP (Weeks 1-4)
- [ ] Product URL → Brand DNA analysis
- [ ] Competitor ad research (Facebook Ad Library scraper)
- [ ] AI ad copy generation (qwen3.5-plus)
- [ ] Simple dashboard

### Phase 2: Creative Generation (Weeks 5-8)
- [ ] Product image extraction
- [ ] Ad creative templates (Freepik integration)
- [ ] Image compositing (product + background)
- [ ] Multi-variation generator

### Phase 3: Publishing (Weeks 9-12)
- [ ] Facebook Ads API integration
- [ ] Campaign creation flow
- [ ] Performance tracking
- [ ] Auto-optimization rules

### Phase 4: Multi-Channel (Months 4-6)
- [ ] Instagram integration
- [ ] TikTok Ads API
- [ ] Google Ads integration
- [ ] Email marketing (Klaviyo/SendGrid)
- [ ] Social media scheduling

---

## 🏗️ Tech Stack

### Frontend
- **Framework:** Next.js 15
- **UI:** Tailwind CSS + shadcn/ui
- **Deployment:** Vercel

### Backend
- **Runtime:** Node.js 22
- **Database:** PostgreSQL (Supabase or self-hosted)
- **Cache:** Redis (Upstash)
- **Queue:** Bull (job processing)

### AI
- **Primary:** Bailian qwen3.5-plus (1M context, $0 cost)
- **Images:** Freepik API (existing subscription)
- **Fallback:** qwen3-coder-plus for code tasks

### Integrations
- Facebook Marketing API
- Facebook Ad Library (scraping)
- TikTok Ads API
- Google Ads API
- Shopify API (product sync)
- Stripe (payments)

---

## 💰 Business Model

### Pricing Tiers

| Tier | Price | Target | Features |
|------|-------|--------|----------|
| **Starter** | $49/mo | Solopreneurs | 10 ads/month, 1 channel, basic templates |
| **Pro** | $149/mo | Growing brands | Unlimited ads, 3 channels, auto-optimization |
| **Agency** | $299/mo | Marketing agencies | White-label, 10 clients, API access |

### Revenue Projections

| Timeline | Customers | MRR | Notes |
|----------|-----------|-----|-------|
| Month 1-3 | 50 | $2.5K | Beta, early adopters |
| Month 4-6 | 200 | $20K | Product Hunt launch |
| Month 7-12 | 1,000 | $100K | Content marketing |
| Year 2 | 5,000 | $500K | Agency tier scales |

**Exit potential:** $5-10M at $1M ARR (5-10x SaaS multiple)

---

## 📁 Project Structure

```
ai-marketing-platform/
├── apps/
│   ├── web/              # Next.js frontend
│   ├── api/              # Backend API (Node.js)
│   └── worker/           # Background jobs (Bull)
├── packages/
│   ├── db/               # Database schema (Prisma)
│   ├── ai/               # AI utilities (Bailian SDK)
│   └── integrations/     # Facebook, TikTok, Google APIs
├── skills/               # OpenClaw skills for automation
├── docs/                 # Documentation
└── scripts/              # Deployment scripts
```

---

## 🎯 Competitive Advantages

1. **$0 model cost** - Bailian models are free for us (vs. GPT-4 costs)
2. **1M context window** - Analyze entire brand + all campaigns at once
3. **Freepik integration** - Real stock photos, not AI-mangled images
4. **Multi-channel from day 1** - Not just Facebook
5. **White-label option** - Capture agency market (Crush doesn't have this)

---

## 📋 Development Status

- [x] GitHub repo created (March 17, 2026)
- [x] Reverse engineering complete (Crush.ai, Holo.ai)
- [x] Business model validated
- [ ] MVP scope defined
- [ ] Tech stack finalized
- [ ] First commit

---

## 🔗 Related Docs

- [Crush.ai Reverse Engineering](../../crush-ai-reverse-engineering.md)
- [Bailian Monetization Opportunities](../../bailian-monetization-opportunities.md)

---

## 🚀 Getting Started (Dev)

```bash
# Clone repo
git clone https://github.com/shaidzin/ai-marketing-platform.git
cd ai-marketing-platform

# Install dependencies (TBD)
npm install

# Run dev (TBD)
npm run dev
```

---

## 📞 Next Steps

1. **Finalize MVP scope** - Pick 3-4 core features for Week 1-4
2. **Set up dev environment** - Next.js + Tailwind + shadcn
3. **Build competitor scraper** - Facebook Ad Library
4. **Test AI copy generation** - qwen3.5-plus for ad copy
5. **Design landing page** - Use business factory template

---

**Built with ❤️ using OpenClaw + Bailian AI**

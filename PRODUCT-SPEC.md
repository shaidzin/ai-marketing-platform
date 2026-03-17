# Product Specification: AI Marketing Platform

**Version:** 1.0 (Draft)
**Date:** March 17, 2026
**Status:** Planning

---

## Executive Summary

**Product:** AI-powered marketing automation platform
**Target:** E-commerce brands, DTC founders, small marketing teams
**Price:** $49-$299/month (tiered)
**Launch:** 8-12 weeks to MVP

**Core Value:** Connect your product URL → AI generates + publishes optimized ads across Facebook, Instagram, TikTok, Google, and email.

---

## User Personas

### 1. Sarah - E-commerce Founder
- **Age:** 32
- **Business:** DTC skincare brand ($50K/month revenue)
- **Pain:** Spending $5K/month on agency, results are mediocre
- **Goal:** Reduce costs, improve ROAS, have more control
- **Will pay:** $149/month easily (saves $4.8K/month)

### 2. Mike - Dropshipper
- **Age:** 28
- **Business:** Multiple Shopify stores, testing products
- **Pain:** Creative fatigue, ads stop working after 2 weeks
- **Goal:** Constant fresh creatives, fast testing
- **Will pay:** $49/month (low risk, high value)

### 3. Emma - Marketing Agency Owner
- **Age:** 38
- **Business:** 15 clients, $80K/month retainer revenue
- **Pain:** Hiring designers/copywriters is expensive
- **Goal:** Scale without adding headcount
- **Will pay:** $299/month white-label (serves 10 clients)

---

## Core Features (MVP)

### Feature 1: Brand DNA Analysis
**User Story:** As a user, I want to paste my product URL so the AI understands my brand automatically.

**Flow:**
1. User enters product URL (e.g., `https://mystore.com/serum`)
2. System scrapes: product images, description, price, reviews
3. qwen3.5-plus analyzes: brand voice, target audience, pain points, USPs
4. Output: Brand DNA profile (saved for future campaigns)

**Tech:**
- Browser automation (Chrome CDP) for scraping
- qwen3.5-plus (1M context) for analysis
- Store in PostgreSQL

**Time:** 3-4 days

---

### Feature 2: Competitor Ad Research
**User Story:** As a user, I want to see what ads my competitors are running so I can model winning creatives.

**Flow:**
1. User enters competitor domains (or keywords)
2. System scrapes Facebook Ad Library
3. Shows: active ads, creative, copy, estimated reach, days running
4. "Winning ads" highlighted (long-running = profitable)
5. One-click: "Generate similar ad for my brand"

**Tech:**
- Facebook Ad Library scraping (browser automation)
- Store ads in DB for quick retrieval
- qwen3.5-plus to analyze patterns

**Time:** 5-7 days

---

### Feature 3: AI Ad Copy Generator
**User Story:** As a user, I want AI to write ad copy in my brand voice so I don't have to write it myself.

**Flow:**
1. System uses Brand DNA (from Feature 1)
2. User selects: ad type (awareness, conversion, retargeting)
3. qwen3.5-plus generates: 10 variations of ad copy
4. User picks favorites, edits if needed
5. Save to campaign

**Tech:**
- qwen3.5-plus with custom system prompt
- Prompt includes: brand voice, audience, product details
- Store variations in DB

**Time:** 2-3 days

---

### Feature 4: Creative Template System
**User Story:** As a user, I want to apply my product images to proven ad templates so I get high-converting creatives fast.

**Flow:**
1. System extracts product images (from Feature 1)
2. User browses template library (Freepik)
3. System composites: product + template background
4. Generates: 10-20 variations (different backgrounds, layouts)
5. Preview in ad mockup (Facebook feed, Instagram story, etc.)

**Tech:**
- Freepik browser automation (or API)
- Sharp.js for image compositing
- Canva-like editor (basic) for adjustments

**Time:** 7-10 days

---

### Feature 5: Campaign Builder
**User Story:** As a user, I want to assemble my ad (copy + creative) and launch it to Facebook with one click.

**Flow:**
1. User selects: copy variation + creative variation
2. Sets: budget, audience, duration
3. System creates Facebook campaign via API
4. Shows: confirmation, campaign ID, estimated reach
5. Dashboard: live performance metrics

**Tech:**
- Facebook Marketing API
- OAuth flow for Facebook connection
- Campaign creation endpoint
- Performance polling (every 6 hours)

**Time:** 10-14 days (Facebook API approval required)

---

### Feature 6: Auto-Optimization Engine
**User Story:** As a user, I want the AI to automatically optimize my campaigns so I don't have to monitor them daily.

**Flow:**
1. System monitors campaign performance (ROAS, CTR, CPC)
2. Rules engine evaluates:
   - If ROAS > 3x → increase budget 20%
   - If ROAS < 1x after 3 days → pause ad
   - If CTR < 1% → suggest new creative
3. User gets: daily summary email, alerts for major changes
4. User can: override rules, set custom thresholds

**Tech:**
- Bull queue (scheduled jobs)
- Facebook Ads Insights API
- Rules engine (configurable)
- Email notifications (Resend/SendGrid)

**Time:** 7-10 days

---

## Technical Architecture

### High-Level Diagram

```
┌─────────────────┐
│   Next.js App   │  ← User interacts
│   (Vercel)      │
└────────┬────────┘
         │
         ↓
┌─────────────────┐
│   API Server    │  ← Node.js + Express
│   (Railway)     │
└────────┬────────┘
         │
    ┌────┴────┬──────────┬─────────────┐
    ↓         ↓          ↓             ↓
┌───────┐ ┌───────┐ ┌─────────┐ ┌──────────┐
│Postgres│ │Redis  │ │Bailian  │ │Facebook  │
│(DB)   │ │(Cache)│ │AI API   │ │Ads API   │
└───────┘ └───────┘ └─────────┘ └──────────┘
```

### Database Schema (Prisma)

```prisma
model User {
  id        String   @id @default(uuid())
  email     String   @unique
  plan      String   @default("starter") // starter, pro, agency
  createdAt DateTime @default(now())
  brands    Brand[]
  campaigns Campaign[]
}

model Brand {
  id          String   @id @default(uuid())
  userId      String
  user        User     @relation(fields: [userId], references: [id])
  name        String
  url         String
  brandDna    Json     // AI-generated profile
  products    Product[]
  createdAt   DateTime @default(now())
}

model Product {
  id          String   @id @default(uuid())
  brandId     String
  brand       Brand    @relation(fields: [brandId], references: [id])
  name        String
  url         String
  images      String[]
  description String
  price       Float
  ads         Ad[]
}

model Ad {
  id          String   @id @default(uuid())
  productId   String
  product     Product  @relation(fields: [productId], references: [id])
  copy        String
  creativeUrl String
  platform    String   // facebook, instagram, tiktok, google
  status      String   // draft, active, paused, completed
  metrics     Json     // performance data
  campaigns   Campaign[]
}

model Campaign {
  id          String   @id @default(uuid())
  userId      String
  user        User     @relation(fields: [userId], references: [id])
  platformId  String   // Facebook campaign ID
  budget      Float
  status      String
  ads         Ad[]
  createdAt   DateTime @default(now())
}
```

---

## API Endpoints (MVP)

### Authentication
- `POST /api/auth/login` - Email magic link
- `POST /api/auth/logout` - Logout

### Brands
- `GET /api/brands` - List user's brands
- `POST /api/brands` - Create brand (from URL)
- `GET /api/brands/:id` - Get brand details
- `DELETE /api/brands/:id` - Delete brand

### Competitor Research
- `POST /api/competitors/scan` - Scan competitor domain
- `GET /api/competitors/:brandId` - Get competitor ads

### Ads
- `POST /api/ads/generate` - Generate ad copy + creative
- `GET /api/ads/:id` - Get ad details
- `PUT /api/ads/:id` - Update ad
- `POST /api/ads/:id/publish` - Publish to platform

### Campaigns
- `GET /api/campaigns` - List campaigns
- `POST /api/campaigns` - Create campaign
- `GET /api/campaigns/:id` - Get campaign + metrics
- `POST /api/campaigns/:id/optimize` - Trigger optimization

---

## Integrations Required

### Must-Have (MVP)
1. **Facebook Marketing API** - Campaign creation, insights
2. **Facebook Ad Library** - Competitor research (scraping)
3. **Bailian API** - AI copy generation (already configured)
4. **Freepik** - Image templates (browser automation)
5. **Stripe** - Payments

### Nice-to-Have (Post-MVP)
1. **TikTok Ads API** - Expand channels
2. **Google Ads API** - Search + Display
3. **Shopify API** - Auto-sync products
4. **Klaviyo** - Email marketing
5. **Canva API** - Advanced creative editing

---

## Go-to-Market Strategy

### Pre-Launch (Week 1-4)
- Build waitlist landing page
- Post on Indie Hackers, r/ecommerce, r/dropshipping
- DM 50 e-commerce founders (Twitter, LinkedIn)
- Goal: 100 beta signups

### Launch (Week 5-8)
- Product Hunt launch
- Offer: 50% off first 3 months ($25/month)
- Collect testimonials from beta users
- Goal: 50 paying customers

### Post-Launch (Month 3-6)
- Content marketing (blog: Facebook ads tips)
- TikTok/Reels: Show before/after results
- Partner with e-commerce influencers
- Goal: 500 customers

---

## Success Metrics

### Product Metrics
- **Activation rate:** % who create first ad (target: 60%)
- **Time to first ad:** Minutes from signup to published ad (target: <10 min)
- **Weekly active:** % using platform weekly (target: 70%)
- **Churn:** Monthly cancellation rate (target: <5%)

### Business Metrics
- **MRR:** Monthly recurring revenue
- **CAC:** Customer acquisition cost (target: <$100)
- **LTV:** Lifetime value (target: >$1,000)
- **LTV/CAC:** Ratio (target: >10x)

---

## Risks & Mitigation

| Risk | Likelihood | Impact | Mitigation |
|------|------------|--------|------------|
| Facebook API approval delayed | Medium | High | Start approval process Week 1, use manual export as fallback |
| AI copy quality poor | Low | Medium | Fine-tune prompts, add human review tier |
| Image quality issues | Medium | Medium | Use Freepik templates (not AI generation), manual override |
| Low conversion from free trial | Medium | High | Onboarding emails, demo videos, live support |
| Competitor copies us | High | Low | First-mover advantage, community building, faster iteration |

---

## Timeline

| Week | Milestone |
|------|-----------|
| 1-2 | Brand DNA + competitor scraper |
| 3-4 | Ad copy generator + template system |
| 5-6 | Facebook API integration |
| 7-8 | Campaign builder + dashboard |
| 9-10 | Auto-optimization engine |
| 11-12 | Beta testing, bug fixes |
| 13 | **Launch on Product Hunt** |

---

## Open Questions

1. **Facebook API approval:** How long does it take? What's required?
2. **Pricing validation:** Is $49/$149/$299 the right tiers?
3. **Image editing:** Build in-house or integrate Canva/Photopea?
4. **Multi-tenant:** Support agencies from day 1 or add later?
5. **White-label:** How much customization is needed for agency tier?

---

**Next Steps:**
1. [ ] Start Facebook API approval process
2. [ ] Build landing page + waitlist
3. [ ] Implement Brand DNA scraper
4. [ ] Test qwen3.5-plus for ad copy quality
5. [ ] Design template library (Freepik integration)

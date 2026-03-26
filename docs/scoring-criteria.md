# ICP Scoring

Framework for scoring and prioritising leads automatically. The system evaluates every lead across five dimensions, assigns a score out of 100, and routes them into the appropriate outbound track.

---

## The Five Dimensions

### 1. Industry Fit (25 points)

How closely the company's industry matches the target market. Companies in the core vertical score highest. Adjacent industries receive partial credit. A perfect industry match gets the full 25 points. A related space might score 10-15. A completely mismatched industry scores zero. This dimension is the primary filter: if the industry does not match, the remaining dimensions are not relevant.

### 2. Company Size (25 points)

There is an optimal range. Companies that are too small lack the budget or need. Companies that are too large have enterprise procurement processes and existing vendor relationships that extend timelines significantly.

The ideal range is companies large enough to have the problem and the budget to solve it, but small enough to make a decision without a multi-month committee process.

### 3. Buying Stage (25 points)

Where the company is in their growth journey. Companies actively scaling their go-to-market motion are the strongest buyers: hiring, expanding into new territories, or launching new products. They need infrastructure to support that growth.

Early-stage companies are typically still establishing product-market fit. Late-stage enterprises have already made vendor decisions and locked in contracts. The middle stage is where conversion rates are highest.

### 4. Revenue Signal (15 points)

Revenue indicates buying power. Companies in a specific revenue range have sufficient budget to invest but have not yet reached the scale where enterprise-grade tooling is already in place.

This dimension carries less weight than the first three because revenue alone does not predict fit. A high-revenue company in the wrong industry is still a poor lead. Combined with the other signals, revenue helps separate serious buyers from aspirational ones.

### 5. Tech Stack Signal (10 points)

The tools a company already uses indicate their readiness. Certain platforms indicate investment in sales infrastructure and openness to complementary tools.

If a company runs a modern CRM, uses outbound tools, and has marketing automation in place, they understand the category. The sales conversation can focus on differentiation rather than category education.

---

## The Tier System

Once a lead is scored across all five dimensions, the total determines their tier.

**Tier 1 (75-100 points): Priority Outbound.** Best-fit companies matching on industry, size, buying stage, and signals. These receive the most personalised outreach, the fastest follow-up, and the highest attention. Every Tier 1 lead should receive outreach that references something specific to their situation.

**Tier 2 (55-74 points): Standard Outbound.** Good fit but not perfect. The company size may be slightly outside the optimal range, or buying stage signals are mixed. They receive well-crafted sequences but do not take priority over Tier 1.

**Tier 3 (35-54 points): Nurture.** These leads have potential but the timing or fit is not right currently. They go into a lower-frequency nurture track. Re-evaluate in 3-6 months.

**Disqualified (0-34 points): Do Not Pursue.** Poor fit on industry, size, or stage. Pursuing these leads wastes time and damages sender reputation.

---

## How AI Applies This Automatically

The scoring criteria lives in a strategy document that the AI orchestration layer reads directly. When instructed to "score these 200 companies against our ICP," the AI reads the rubric, evaluates each company across all five dimensions, assigns a total score and tier, and outputs a ranked list.

The process that previously required manual spreadsheet evaluation runs in seconds and applies the same standard consistently across every company in the list.

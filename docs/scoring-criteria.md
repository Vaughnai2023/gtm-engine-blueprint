# ICP Scoring — How We Prioritise Leads

Not all leads are equal. This document explains the framework I built to score and prioritise companies automatically. The system evaluates every lead across five dimensions, assigns a score out of 100, and routes them into the right outbound track.

---

## The Five Dimensions

### 1. Industry Fit (25 points)

How closely does the company's industry match our target market? Companies in our core vertical score highest. Adjacent industries get partial credit. A company that is a perfect industry match gets the full 25 points. A company in a related space might get 10-15. A completely mismatched industry scores zero. This is the first filter. If the industry is wrong, nothing else matters.

### 2. Company Size (25 points)

There is a sweet spot. Too small and they don't have the budget or need. Too large and they have enterprise procurement processes and existing vendor relationships that take months to navigate.

The ideal range is companies big enough to have the problem but small enough to move fast. They feel the pain, they have budget to solve it, and they can make a decision without a 6-month committee process.

### 3. Buying Stage (25 points)

Where is the company in their growth journey? Companies actively scaling their go-to-market motion are the best buyers. They are hiring, expanding into new territories, or launching new products. They need infrastructure to support that growth.

Early-stage companies aren't ready yet. They are still figuring out product-market fit. Late-stage enterprises have already made their decisions and locked in vendor contracts. The middle is where the action is.

### 4. Revenue Signal (15 points)

Revenue indicates buying power. Companies in a specific revenue range have enough budget to invest but aren't so large that they have enterprise-grade tooling already in place.

This dimension carries less weight than the first three because revenue alone doesn't predict fit. A high-revenue company in the wrong industry is still a bad lead. But when combined with the other signals, revenue helps separate serious buyers from aspirational ones.

### 5. Tech Stack Signal (10 points)

What tools a company already uses tells you a lot about their readiness. Certain platforms indicate they are investing in sales infrastructure and are likely open to complementary tools.

If a company runs a modern CRM, uses outbound tools, and has marketing automation in place, they understand the category. They are easier to sell to because you don't have to educate them on why the category exists. You just have to show them why your approach is better.

---

## The Tier System

Once a lead is scored across all five dimensions, the total determines their tier.

**Tier 1 (75-100 points): Priority Outbound.** These are the best-fit companies. They match on industry, size, buying stage, and signals. They get the most personalised outreach, the fastest follow-up, and the most attention. Every Tier 1 lead should feel like the email was written just for them.

**Tier 2 (55-74 points): Standard Outbound.** Good fit but not perfect. Maybe the company size is slightly outside the sweet spot, or the buying stage signals are mixed. They still get a well-crafted sequence, but they don't jump the queue ahead of Tier 1.

**Tier 3 (35-54 points): Nurture.** These leads have potential but the timing or fit isn't right yet. They go into a lower-frequency nurture track. Check back in 3-6 months. Things change.

**Disqualified (0-34 points): Do Not Pursue.** Bad fit. Wrong industry, wrong size, wrong stage. Chasing these leads wastes time and damages sender reputation. Move on.

---

## How AI Applies This Automatically

The scoring criteria lives in a strategy document that the AI orchestration layer reads directly. When I say "score these 200 companies against our ICP," the AI reads the rubric, evaluates each company across all five dimensions, assigns a total score and tier, and outputs a ranked list.

What used to take hours of manual spreadsheet work happens in seconds. And it is consistent. The AI doesn't get tired at row 150 and start giving generous scores. Every company gets evaluated against the same standard, every time.

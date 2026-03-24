# GTM Engine Blueprint

**A complete go-to-market engine I built from scratch for a UK healthtech company launching an AI product into the NHS.**

15 automation workflows. AI-powered pipeline management. An orchestration layer that deploys full outbound campaigns in under 5 minutes. One person. Zero existing pipeline.

<!-- ![System Overview](screenshots/workflow-list.png) -->

---

## The Problem

Here's what I walked into:

- A brand-new AI product with **zero pipeline** and **zero brand awareness**
- A market (NHS) that is notoriously hard to sell into — long buying cycles, multi-stakeholder sign-off, strict information governance
- A founding GTM role — meaning I build everything: strategy, systems, automation, outbound, content, pilot management

The product solves a real problem. The NHS needs 6,500 link workers by end of decade (up from 3,500 today). There aren't enough people to hire. This AI product replaces the bottleneck — it identifies, triages, and connects patients to community services around the clock. No recruitment needed. No waitlists.

But a great product doesn't sell itself. Especially not in the NHS.

**I needed to build a GTM engine that could find the right people, reach them with the right message, handle every response intelligently, and manage pilots from sign-up to case study — mostly on autopilot.**

---

## What I Built

The system has four parts. Each one works on its own. Together, they create a flywheel where every output feeds the next input.

---

### 1. GTM Strategy

> [Read the full strategy document](docs/strategy.md)

Before writing a single email or building a single workflow, I mapped out the entire go-to-market plan. This isn't a slide deck — it's an execution playbook.

**Who we're selling to (ICP Definition):**

I defined three tiers of buyers based on who has the most pain and the shortest path to a deal:

| Tier | Who | Why They Buy |
|------|-----|-------------|
| **Tier 1** | ICB Commissioners & PCN Clinical Directors | Can't hire enough link workers. Referral backlogs growing. Need outcome data they can't produce. |
| **Tier 2** | Local Authority Public Health Teams | Budget cuts meeting rising demand. Need to prove prevention ROI. |
| **Tier 3** | VCSE Social Prescribing Providers | Overwhelmed staff, growing waitlists, can't scale. |

**How we reach them (5 Channels):**

Not all channels are equal. I prioritised them by speed-to-revenue:

1. **Warm network** — The parent company has 1,200+ GP surgeries. That's the unfair advantage. Start here.
2. **Cold email + LinkedIn** — Signal-triggered, personalised outreach to Tier 1 decision-makers
3. **LinkedIn content** — Founder-led posts that build awareness and capture engagement signals
4. **WhatsApp** — NHS circles use WhatsApp heavily. High-trust, low-noise follow-up channel.
5. **NHS events** — Attend, speak, capture contacts, follow up within 24 hours

**How we find the right moment (Signal Intelligence):**

Instead of blasting cold emails, I built a system that watches for buying signals — things that tell us an organisation is ready to buy *right now*:

| Signal | What It Means | Response Time |
|--------|--------------|---------------|
| Link worker vacancy open 30+ days | They can't hire. Budget exists. Need is proven. | Within 24 hours |
| ICB board paper mentions workforce challenges | Leadership is aware of the problem. | Within 48 hours |
| New Clinical Director appointed | New leaders look for quick wins. 90-day window. | Within 1 week |
| Competitor contract approaching renewal | They're already evaluating options. | Within 1 week |
| ICP engages with our LinkedIn content | They already know who we are. Warm signal. | Same day |

**The pipeline model:**

NHS doesn't follow standard SaaS funnel maths. I built a realistic model:

```
Demo → Champion Identified (60%)
  → Internal Socialisation Complete (50%)
    → IG/Procurement Approved (40%)
      → Pilot Signed (70%)
```

Working backwards from 3-5 pilots: need 35-50 demos, 20-30 champions, 10-15 socialised opportunities.

**Plus:** A complete 30/60/90 day execution roadmap, metrics framework, pricing strategy, and email infrastructure plan.

---

### 2. Automation Engine — 15 Workflows

> [Read the full architecture document](docs/automation-architecture.md)

I designed and built 15 interconnected automation workflows that handle everything from finding leads to managing pilots. The system runs mostly on autopilot — the team's main job is responding to interested replies and running demos.

Here's what the system looks like:

```
┌─────────────────────────────────────────────────────────────────┐
│                    GTM AUTOMATION SUITE                          │
├─────────────────────────────────────────────────────────────────┤
│                                                                  │
│  SIGNAL INTELLIGENCE          ENGAGEMENT PIPELINE               │
│  ┌──────────────────┐         ┌──────────────────┐              │
│  │ NHS Jobs Monitor  │         │ LinkedIn Capture  │             │
│  │ Board Papers      │         │ Content Flywheel  │             │
│  │ Competitor Watch   │         │ Engagement Routes │             │
│  │ Leadership Track   │         │                   │             │
│  └────────┬─────────┘         └────────┬──────────┘             │
│           │                            │                         │
│           ▼                            ▼                         │
│  ┌──────────────────────────────────────────────────┐           │
│  │          SHARED OUTPUT LAYER                      │           │
│  │   CRM  ·  Email Campaigns  ·  Slack  ·  Sheets   │           │
│  └──────────────────────────────────────────────────┘           │
│                         │                                        │
│                         ▼                                        │
│  ┌──────────────────────────────────────────────────┐           │
│  │          REPLY MANAGEMENT                         │           │
│  │   AI classifies every reply → 6 different actions │           │
│  └──────────────────────────────────────────────────┘           │
│                         │                                        │
│                         ▼                                        │
│  ┌──────────────────────────────────────────────────┐           │
│  │          PILOT LIFECYCLE                          │           │
│  │   Onboarding → Health Checks → Milestones → Case │           │
│  │   Study                                           │           │
│  └──────────────────────────────────────────────────┘           │
│                                                                  │
│  INFRASTRUCTURE: Error handler watches everything               │
│                                                                  │
└─────────────────────────────────────────────────────────────────┘
```

**The five workflow groups:**

**Signal Intelligence (4 workflows)**

These run on schedules — every Monday, Wednesday, Friday, or bi-weekly — and scan the NHS landscape for buying signals:

- **NHS Jobs Monitor** — Checks for link worker vacancies every Monday at 6am. If a role has been open 30+ days, that's a strong signal the organisation can't hire. The lead gets enriched and pushed to outbound.
- **Board Paper Monitor** — Every Wednesday, AI reads ICB board paper excerpts looking for workforce challenges, social prescribing expansion plans, or digital transformation mentions. High-urgency findings get flagged to the team.
- **Competitor Contract Monitor** — Every two weeks, checks NHS procurement portals for competitor contracts approaching renewal. The best time to win a customer is when their current vendor's deal is about to expire.
- **Leadership Appointment Monitor** — Every Friday, watches for new Clinical Directors and Digital Leads. New leaders have a 90-day window where they're actively looking for quick wins. If the parent company already operates in their area, the team gets a warm intro opportunity.

**Engagement Pipeline (4 workflows)**

These capture people who interact with our LinkedIn content and turn them into leads:

- Real-time capture: When an ICP-matching decision-maker likes, comments, or shares a post, they're immediately enriched and scored
- Daily batch: Every evening, the full day's engagement gets processed in one sweep
- Smart routing: High-ICP-score leads get personalised outbound. Lower scores get softer marketing touches. Nobody falls through the cracks.

**Reply Management (1 workflow — the showcase piece)**

<!-- ![Reply Handler](screenshots/reply-handler.png) -->

This is the workflow I'm most proud of. When someone replies to a cold email, AI reads the reply and classifies it into one of six categories. Then it takes completely different actions for each:

| Reply Type | What Happens Automatically |
|-----------|---------------------------|
| **Interested** | Urgent Slack alert. Deal created in CRM. Removed from email sequence. |
| **Objection** | Team notified for manual response. Sequence paused. |
| **Not Now** | Tagged for re-engagement in 60 days. Removed from current campaign. |
| **Wrong Person** | Their company gets re-enriched. The *actual* decision-maker gets found and added to a new campaign. |
| **Unsubscribe** | Removed from everything. Marked "Do Not Contact." No exceptions. |
| **Auto-Reply** | No action. Sequence continues normally. |

The "Wrong Person" branch is particularly powerful — instead of losing a lead, it automatically finds the right person at the same organisation.

**Pilot Lifecycle (4 workflows)**

These manage everything from the moment a client signs a pilot deal:

- **Kickoff** — Automatically sets milestones, sends a welcome pack, creates a health dashboard
- **Weekly Health Check** — Every Friday, pulls pilot metrics, compares against benchmarks, alerts the team if anything's below target, sends the client a weekly summary
- **Milestone Triggers** — Strategically timed emails: Week 4 (first outcomes review), Week 8 (expansion conversation starts), Week 10 (internal alert to prepare proposal), Week 12 (final review + expansion offer)
- **Completion** — AI generates a case study draft from the pilot data. The deal moves to "Expansion Opportunity."

**Error Handler (1 workflow)**

The safety net. If any workflow breaks — an API is down, data arrives in an unexpected format — the team gets a clear Slack message explaining what went wrong, which workflow failed, and when.

---

### 3. AI-Powered Campaign Orchestration

> [Read about the orchestration approach](docs/scoring-criteria.md) and [email methodology](docs/copy-framework.md)

This is where it gets interesting. I built an AI orchestration layer using Claude Code that turns campaign deployment from a multi-hour process into a 5-minute conversation.

**The concept:**

A single instruction file tells Claude Code everything it needs to know — which tools to use, which strategy documents to read, which rules to follow, and in what order. The result is an AI that acts as a GTM operator.

**The 5-step pipeline:**

```
Step 1: Score Companies
  "Score these 200 companies against our ICP"
  → Reads the scoring rubric, scores across 5 dimensions,
    assigns tiers (T1/T2/T3/Disqualified)

Step 2: Find Decision-Makers
  "Find VP-level contacts at our Tier 1 companies"
  → Searches for matching contacts, returns persona breakdown

Step 3: Get Contact Info
  "Find work emails for all contacts"
  → Enriches contacts with verified work emails

Step 4: Write Email Sequences
  "Write a 3-email sequence using the workforce gap angle"
  → Reads the copy framework, generates on-brand sequences
    following word count, tone, and CTA rules

Step 5: Deploy Campaign
  "Load this into our email platform as a draft"
  → Creates the campaign with sequences, loads all leads,
    sets sending schedule — ready for human review
```

**Why this matters:**

Without this layer, each campaign requires jumping between 5 different tools, manually copying data, and spending 2-3 hours on setup. With it, you describe what you want in plain English and the AI handles the rest.

The scoring criteria and copy framework act as the "source of truth" — change the documents and the AI adapts. No code changes needed. It's like having a junior GTM operator who reads your playbook perfectly every time.

**ICP Scoring Framework:**

I built a quantified scoring system across 5 dimensions (100 points total):

1. **Industry Fit** (25 pts) — How well does this company match our target market?
2. **Company Size** (25 pts) — Are they in the sweet spot for our product?
3. **Buying Stage** (25 pts) — Are they at a stage where they're likely to invest?
4. **Revenue Signal** (15 pts) — Does their revenue indicate buying power?
5. **Tech Stack** (10 pts) — Are they already using tools that signal readiness?

Companies score into tiers: T1 (75-100) gets priority outbound, T2 (55-74) gets standard sequences, T3 (35-54) goes to nurture, and anything below 35 is disqualified. This means the team only spends time on companies most likely to buy.

**Email Copy Methodology:**

Every outbound email follows a strict framework:

- **70-90 words per email** — Short enough to read on a phone
- **Peer-to-peer tone** — Like texting a colleague, not pitching a product
- **Soft CTAs only** — "Worth a look?" not "Book a demo"
- **3-5 word subject lines, lowercase** — Should look like an internal email
- **Plain text only** — No HTML, no images, no tracked links (better deliverability)
- **One job per email** — Each email makes one point and one ask

---

### 4. Pilot Success System

> [Read the full pilot playbook](docs/pilot-playbook.md)

In NHS, the first pilot determines everything. A successful pilot becomes a case study, which becomes proof for the next buyer, which becomes content that attracts more leads. A failed pilot kills the flywheel.

I built a system that treats every pilot as a case study in the making:

**Pre-Pilot (2 weeks before launch):**
- IG (Information Governance) compliance checklist — DTAC assessment, DPIA, data processing agreements
- Technical integration with GP systems
- Stakeholder alignment — Clinical Director briefing, existing staff orientation, success metrics agreed

**12-Week Pilot Execution:**

| Week | What Happens | Why It's Timed This Way |
|------|-------------|------------------------|
| 1-2 | Soft launch on limited patient cohort. Daily monitoring. | Catch issues early before scaling. |
| 3-4 | Scale to full pilot scope. First outcomes review. | Prove value fast. Capture first testimonial. |
| 5-8 | Steady state. Bi-weekly reviews. Start drafting case study. | Build evidence while data is accumulating. |
| 8 | Mid-pilot review. **Start the expansion conversation.** | Not Week 12 when it feels rushed. Week 8, when results are clear. |
| 10 | Internal alert: prepare expansion proposal. | Give the team 2 weeks to build a compelling offer. |
| 12 | Final review. Present expansion options. AI generates case study. | Close the loop while momentum is highest. |

**Pilot Metrics Dashboard:**

| Metric | Target | Measured |
|--------|--------|---------|
| Referrals processed/week | 2x baseline | Weekly |
| Patient contact within 48 hours | 90%+ | Weekly |
| GP appointment reduction | 15-20% | Monthly |
| Patient well-being improvement | 70%+ | End of pilot |
| Cost per referral | 50% reduction vs. human | End of pilot |

**The automation handles:**
- Onboarding kickoff (welcome pack, timeline, checklists)
- Weekly health checks (metrics vs. benchmarks, early warning alerts)
- Milestone emails (strategically timed for expansion conversations)
- Case study generation (AI drafts from pilot data on completion)

---

## The Flywheel

Here's how all four parts connect into a system that gets stronger over time:

```
Warm network generates first demos + proof points
        ↓
LinkedIn content creates awareness + captures engagement signals
        ↓
Signals feed outbound targeting (warm prospects get priority)
        ↓
Outbound drives demo conversations
        ↓
Demo insights fuel better content (objections become posts)
        ↓
Wins become pilots
        ↓
Pilot data becomes case studies
        ↓
Case studies power more content → more signals → cycle accelerates
```

Every output feeds the next input. The system gets smarter and more effective with each cycle.

---

## Results Framework

All 15 workflows are structurally complete and demo-ready. The metrics below represent what this system is designed to achieve:

| Metric | Target |
|--------|--------|
| Emails sent/day | 50-75 (by Month 2) |
| Open rate | 45%+ |
| Positive reply rate | 5-8% |
| Demos booked/month | 15-20 |
| Champions identified | 6-10 by Day 60 |
| Pilots signed | 2-3 by Day 90 |
| Pipeline value | £200K+ by Day 90 |
| Signal-to-outreach time | Under 24 hours for Tier 1 signals |
| Time to deploy a campaign | Under 5 minutes (with AI orchestration) |

---

## Tech Stack

The entire system runs on a lean, cost-effective stack:

| Layer | Tool | Role | Monthly Cost |
|-------|------|------|-------------|
| Data & Enrichment | Clay | Lead scoring, enrichment, personalisation | ~£100 |
| Orchestration | n8n | Workflow automation, signal routing, reply handling | Free-£20 |
| Email Delivery | Instantly | Sending, warmup, inbox rotation | ~£80 |
| CRM | HubSpot (Free) | Pipeline tracking, deal management | £0 |
| Social | LinkedIn | Content, engagement, manual outreach | £0 |
| AI Orchestration | Claude Code | Campaign deployment, copy generation | ~£20 |
| Signal Capture | Trigify | LinkedIn engagement detection | ~£50 |
| **Total** | | | **~£270-350/month** |

That's a full GTM engine — signal intelligence, automated outbound, AI reply handling, and pilot management — for under £350/month.

---

## Documentation

Each part of this system is documented in detail:

| Document | What's Inside |
|----------|--------------|
| [GTM Strategy](docs/strategy.md) | ICP definition, channel strategy, signal intelligence, pipeline model, 30/60/90 roadmap, pricing, metrics |
| [Automation Architecture](docs/automation-architecture.md) | All 15 workflows explained in plain English — what they do, why they matter, how they connect |
| [Pilot Playbook](docs/pilot-playbook.md) | Pre-pilot checklists, 12-week execution plan, metrics dashboard, expansion playbook |
| [ICP Scoring Approach](docs/scoring-criteria.md) | 5-dimension scoring framework, tier thresholds, and how AI applies it automatically |
| [Email Methodology](docs/copy-framework.md) | Writing rules, sequence structure, and the principles behind high-performing cold email |

---

## About

This GTM engine was designed for a UK healthtech company launching an AI-powered product into the NHS social prescribing market. The system covers everything from finding the first lead to generating a case study after a successful pilot — built to be operated by a single founding GTM hire working directly with the founder.

Want to talk about GTM strategy, automation, or how I'd build something like this for your company? Reach out.

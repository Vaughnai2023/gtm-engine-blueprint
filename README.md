# GTM Engine Blueprint

**A go-to-market engine for a UK healthtech company launching an AI product into the NHS. Built from scratch as a solo operator.**

The system covers signal intelligence, outbound campaigns, reply classification, and pilot lifecycle management across 15 automation workflows. An AI orchestration layer handles campaign deployment in under 5 minutes using natural language commands.

---

## The Problem

- New AI product with no existing pipeline or brand awareness in the target market
- The NHS buying cycle involves multiple stakeholders, strict information governance requirements, and long procurement timelines
- A founding GTM role with sole responsibility for strategy, systems, automation, outbound, content, and pilot management

The requirement was a system that identifies high-intent prospects, reaches them with relevant messaging, classifies and routes every response, and manages pilots from onboarding through case study generation, with minimal manual intervention once configured.

---

## System Overview

> [The ground game](docs/ground-game.md) · [Full GTM strategy](docs/strategy.md) · [Automation architecture](docs/automation-architecture.md) · [Pilot playbook](docs/pilot-playbook.md) · [ICP scoring](docs/scoring-criteria.md)

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

---

## The Workflows

### Signal Intelligence

4 scheduled workflows that scan the NHS landscape for buying signals: job vacancies, board papers, competitor contracts, and leadership changes. When a signal fires, the lead is enriched and pushed to outbound automatically.

![Board Paper Monitor](screenshots/BZ-04.png)
*Board Paper Monitor: AI reads ICB board papers, scores urgency, and routes high-priority signals to the team*

![Competitor Contract Monitor](screenshots/BZ-07.png)
*Competitor Contract Monitor: watches NHS procurement portals for contracts expiring within 90 days*

![Leadership Appointment Monitor](screenshots/BZ-08.png)
*Leadership Appointment Monitor: detects new Clinical Directors and checks for warm intro opportunities*

### Engagement Pipeline

4 workflows that capture LinkedIn engagement in real-time and batch, enrich through Clay, deduplicate in HubSpot, and route by ICP score. Hot signals get priority outbound, warm signals get standard sequences, remaining contacts go to nurture.

![LinkedIn Engagement Capture](screenshots/BZ-02a.png)
*LinkedIn Engagement Capture: real-time Trigify webhook filters for ICP title match and pushes to Clay*

![LinkedIn Engagement, Clay Callback](screenshots/BZ-o2b.png)
*LinkedIn Engagement Clay Callback: deduplicates, scores by ICP fit, and routes to Hot / Warm / Nurture campaigns*

![Content Engagement Flywheel](screenshots/BZ-06a.png)
*Content Engagement Flywheel: daily batch pull from Trigify, filters for ICP titles, pushes to Clay*

![Content Engagement, Clay Callback](screenshots/BZ-06b.png)
*Content Engagement Clay Callback: routes enriched engagers by ICP score to Instantly campaigns or HubSpot nurture*

### Reply Management

When someone replies to a cold email, AI classifies the reply into one of six categories and takes a different action for each:

| Reply Type | Action |
|-----------|---------------------------|
| **Interested** | Urgent Slack alert. Deal created in CRM. Removed from sequence. |
| **Objection** | Team notified for manual response. Sequence paused. |
| **Not Now** | Tagged for re-engagement in 60 days. Removed from campaign. |
| **Wrong Person** | Company re-enriched. The actual decision-maker is found and added to a new campaign. |
| **Unsubscribe** | Removed from everything. Marked "Do Not Contact." |
| **Auto-Reply** | No action. Sequence continues. |

![Reply Handler & Classification](screenshots/BZ-03.png)
*Reply Handler: AI classifies every inbound reply and routes to 6 different action paths*

### Pilot Lifecycle

4 workflows that manage the pilot from signed deal through completion: onboarding kickoff, weekly health checks against benchmarks, milestone-triggered emails, and AI-generated case studies.

![Pilot Onboarding Kickoff](screenshots/BZ-05a.png)
*Pilot Kickoff: triggers on deal stage change, sets milestones, sends welcome email, creates dashboard*

![Weekly Pilot Health Check](screenshots/BZ-05b.png)
*Weekly Health Check: every Friday, AI compares metrics against benchmarks, alerts if below target*

![Pilot Milestone Triggers](screenshots/BZ-05c.png)
*Milestone Triggers: routes to Week 4 outcomes review, Week 8 mid-pilot review, Week 10 expansion prep, or Week 12 final review*

![Pilot Completion & Case Study](screenshots/BZ-05d.png)
*Pilot Completion: AI generates case study from pilot data, moves deal to Expansion Opportunity*

---

## AI Campaign Orchestration

> [ICP scoring approach](docs/scoring-criteria.md)

The AI orchestration layer uses Claude Code to handle campaign deployment through natural language commands. A single instruction file defines which tools to use, which strategy documents to read, and which rules to follow.

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
    sets sending schedule, ready for human review
```

The scoring criteria and copy framework serve as the source of truth. Updating the documents changes the AI's output without code changes.

---

## Results Framework

All 15 workflows are structurally complete and demo-ready. Target metrics:

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

---

## Documentation

| Document | Contents |
|----------|--------------|
| [The Ground Game](docs/ground-game.md) | Week 1 operational playbook: manual outreach, system setup, infrastructure warmup |
| [GTM Strategy](docs/strategy.md) | ICP definition, channel strategy, signal intelligence, pipeline model, 30/60/90 roadmap, pricing, metrics |
| [Automation Architecture](docs/automation-architecture.md) | All 15 workflows: what they do, how they connect, trigger summary |
| [Pilot Playbook](docs/pilot-playbook.md) | Pre-pilot checklists, 12-week execution plan, metrics dashboard, expansion playbook |
| [ICP Scoring Approach](docs/scoring-criteria.md) | 5-dimension scoring framework, tier thresholds, AI application |

---

## Approach

The engine was built in weeks as a solo operator. The typical timeline for standing up a GTM function is 6-12 months. Key design decisions:

- **Speed**: Signal-to-outreach in under 24 hours. Campaign deployment in under 5 minutes. Weekly kill/scale cycles on campaign angles.
- **Buyer respect**: Short emails, soft CTAs, peer-to-peer tone, immediate unsubscribe handling. NHS professionals receive significant vendor noise. The outreach methodology is designed to respect their time.
- **Transparency**: Every decision is documented. Every workflow is explained in plain English. Verification checkpoints are built into every phase to catch problems early.
- **Detail**: 5-dimension scoring rubric. 6-way AI reply classification. Pre-pilot IG compliance checklists.

---

## About

This GTM engine was designed for a UK healthtech company launching an AI-powered product into the NHS social prescribing market. The system covers the full pipeline from lead identification through case study generation, operated through Claude Code using natural language.

# Automation Architecture — 15 Workflows

This document explains every workflow in the GTM automation suite in plain English. Each write-up tells you what the workflow does, why it matters, and how it connects to the rest of the system.

---

## Overview

This is a fully automated go-to-market engine for an AI product in the NHS social prescribing space. It finds leads, qualifies them, runs outreach, handles replies, and manages pilot programmes — all without manual effort beyond responding to interested prospects and running demos.

The suite contains 15 workflows organised into five layers:

- **Signal Intelligence** — monitors that scan the NHS landscape for buying signals
- **Engagement Pipeline** — captures and qualifies people who interact with content
- **Reply Management** — AI-powered classification of every email reply
- **Pilot Lifecycle** — automates onboarding, tracking, and case study generation
- **Infrastructure** — error handling that watches the whole system

---

## Trigger Summary

| Workflow | Trigger Type | Schedule |
|---|---|---|
| WF-00 | Error Trigger | On error from any workflow |
| WF-01a | Schedule (Cron) | Weekly Monday 6:00 AM |
| WF-01b | Webhook | Enrichment callback |
| WF-02a | Webhook | LinkedIn engagement event |
| WF-02b | Webhook | Enrichment callback |
| WF-03 | Webhook | Email reply event |
| WF-04 | Schedule (Cron) | Weekly Wednesday 7:00 AM |
| WF-05a | CRM Trigger | Deal stage moves to "Pilot Signed" |
| WF-05b | Schedule (Cron) | Weekly Friday |
| WF-05c | Schedule (Cron) | Daily check |
| WF-05d | Manual / Webhook | Pilot end trigger |
| WF-06a | Schedule (Cron) | Daily 8:00 PM |
| WF-06b | Webhook | Enrichment callback |
| WF-07 | Schedule (Cron) | Bi-weekly Monday 7:00 AM |
| WF-08 | Schedule (Cron) | Weekly Friday 9:00 AM |

---

## System Map

```
┌─────────────────────────────────────────────────────────────────────────┐
│                    GTM AUTOMATION SUITE                                  │
├─────────────────────────────────────────────────────────────────────────┤
│                                                                         │
│  ┌─── SIGNAL INTELLIGENCE (Cron-Triggered Monitors) ──────────────┐    │
│  │                                                                 │    │
│  │  WF-01a             WF-04              WF-07        WF-08      │    │
│  │  NHS Jobs ──┐       Board Paper        Competitor   Leadership │    │
│  │  Scraper    │       Monitor            Contract     Appointment│    │
│  │     │       │          │               Monitor      Monitor    │    │
│  │     ▼       │          ▼                  │            │       │    │
│  │  Enrichment─┤       AI Analysis           ▼            ▼       │    │
│  │     │       │          │               AI Analysis  Coverage   │    │
│  │     ▼       │          │                  │         Check      │    │
│  │  WF-01b    │          │                  │            │       │    │
│  │  Enrichment─┼──────────┼──────────────────┼────────────┤       │    │
│  │  Callback   │          │                  │            │       │    │
│  └─────────────┼──────────┼──────────────────┼────────────┼───────┘    │
│                │          │                  │            │            │
│  ┌─── ENGAGEMENT PIPELINE ┼──────────────────┼────────────┼───────┐    │
│  │             │          │                  │            │       │    │
│  │  WF-02a    │          │                  │            │       │    │
│  │  LinkedIn ──┤          │                  │            │       │    │
│  │  Capture    │          │                  │            │       │    │
│  │     │       │          │                  │            │       │    │
│  │  WF-02b    │          │                  │            │       │    │
│  │  Callback ──┤          │                  │            │       │    │
│  │             │          │                  │            │       │    │
│  │  WF-06a    │          │                  │            │       │    │
│  │  Content ───┤          │                  │            │       │    │
│  │  Flywheel   │          │                  │            │       │    │
│  │  WF-06b    │          │                  │            │       │    │
│  │  Callback ──┤          │                  │            │       │    │
│  └─────────────┼──────────┼──────────────────┼────────────┼───────┘    │
│                │          │                  │            │            │
│                ▼          ▼                  ▼            ▼            │
│  ┌─── SHARED OUTPUT LAYER ────────────────────────────────────────┐    │
│  │  CRM  ·  Email Platform  ·  Slack  ·  Google Sheets           │    │
│  └────────────────────────────────────────────────────────────────┘    │
│                                                                         │
│  ┌─── REPLY MANAGEMENT ──────────────────────────────────────────┐    │
│  │  WF-03: Reply Handler & Classification                        │    │
│  │  Email Reply → AI Classify → 6-Way Switch                    │    │
│  │  INTERESTED / OBJECTION / NOT_NOW / WRONG_PERSON /            │    │
│  │  UNSUBSCRIBE / AUTO_REPLY                                     │    │
│  └────────────────────────────────────────────────────────────────┘    │
│                                                                         │
│  ┌─── PILOT LIFECYCLE ───────────────────────────────────────────┐    │
│  │  WF-05a Kickoff → WF-05b Health Check → WF-05c Milestones    │    │
│  │  → WF-05d Completion & Case Study                             │    │
│  └────────────────────────────────────────────────────────────────┘    │
│                                                                         │
│  INFRASTRUCTURE: WF-00 Error Handler watches everything                │
│                                                                         │
└─────────────────────────────────────────────────────────────────────────┘
```

---

## Workflow Guide (Plain English)

### WF-00: Error Handler

Safety net for the whole system. If any workflow breaks — API down, unexpected data format — this catches the error and sends a clear Slack message explaining what went wrong, which workflow failed, and when.

---

### WF-01a: NHS Jobs Scraper

Every Monday at 6 AM, checks NHS Jobs for link worker vacancies. If a role has been open 30+ days, that is a signal the organisation cannot hire. High-priority vacancies go to enrichment. Newer ones go to a monitoring list.

**Why it matters:** A long-open vacancy means the organisation is struggling with capacity. That is a strong buying signal for an AI product that supports link workers.

---

### WF-01b: NHS Jobs — Enrichment Callback

When enrichment finishes processing contacts from WF-01a, this workflow handles the results. ICP score 7+? Checks the CRM for duplicates. New contacts get a CRM record, a deal, a cold email campaign, and a Slack notification. Existing contacts are flagged for manual review. Low-scoring contacts are added as marketing leads.

---

### WF-02a: LinkedIn Engagement Capture

Real-time webhook. When someone engages with LinkedIn content (like, comment, share), this checks if their title matches the ICP — Directors, Heads of Service, Clinical Directors, Commissioners, Digital Leads. Matches get pushed to enrichment. Non-ICP titles get skipped.

---

### WF-02b: LinkedIn Engagement — Enrichment Callback

Processes enriched LinkedIn engagers. Checks the CRM: active deal? Skip. Existing contact with no deal? Update with the new signal. New contact with a valid email and ICP score 6+? Add to an email campaign, create a CRM record, and send a Slack notification. Lower-scoring contacts go to nurture.

---

### WF-03: Reply Handler & Classification

**This is the showcase piece of the whole suite.**

When someone replies to a cold email, AI classifies intent into six categories:

- **INTERESTED** — Urgent Slack alert + CRM deal at "Demo Requested" + remove from email sequence
- **OBJECTION** — Slack notification for manual response + CRM tag + pause sequence
- **NOT NOW** — CRM tag for 60-day re-engage + remove from campaign + scheduled re-engagement
- **WRONG PERSON** — Extract company domain, re-enrich, find the actual decision-maker, add them to a new campaign
- **UNSUBSCRIBE** — Remove from all campaigns + mark "Do Not Contact"
- **AUTO REPLY** — No action, sequence continues

The WRONG PERSON branch is particularly powerful. Instead of losing a lead, the system automatically finds the right person at the same organisation.

---

### WF-04: NHS Board Paper Monitor

Every Wednesday at 7 AM, AI analyses ICB board paper excerpts. It looks for organisations struggling with link worker capacity, planning social prescribing expansion, or reviewing digital transformation. Each signal gets an urgency rating from 1 to 10. Score 6+: flagged for enrichment, CRM, and Slack. Lower scores: logged for trend tracking.

---

### WF-05a: Pilot Onboarding Kickoff

Triggers when a CRM deal moves to "Pilot Signed". Updates the deal with pilot dates and milestones (Week 1, 4, 8, 12). Sends a welcome email with timeline, information governance checklist, and technical requirements. Creates a tracking dashboard for weekly health monitoring.

---

### WF-05b: Weekly Pilot Health Check

Every Friday, pulls pilot metrics and compares them against reference benchmarks. Below target? Slack alert. Updates the dashboard. Sends the client a weekly summary with metrics, benchmark comparison, and next week's focus area.

---

### WF-05c: Pilot Milestone Triggers

Daily check: is any pilot hitting a milestone week?

- **Week 4** — First outcomes review request
- **Week 8** — Mid-pilot review + expansion conversation
- **Week 10** — Internal alert to prepare expansion proposal
- **Week 12** — Final review + expansion offer

---

### WF-05d: Pilot Completion & Case Study

When a pilot ends, pulls final metrics. AI generates a structured case study draft covering the challenge, solution, results, and a quote request. The CRM deal moves to "Expansion Opportunity". Team notified on Slack.

---

### WF-06a: Content Engagement Flywheel

Every evening at 8 PM, pulls the full day's LinkedIn engagement. Filters out non-ICP titles. Pushes qualified engagers to enrichment in batch. This is the daily batch version of WF-02a (which captures engagement in real time).

---

### WF-06b: Content Engagement — Enrichment Callback

Routes each lead by ICP score:

- **8-10 (hot)** — Personalised email campaign
- **5-7 (warm)** — Value-first email campaign
- **Below 5** — CRM marketing contact only

Sends a daily Slack summary of all new leads.

---

### WF-07: Competitor Contract Monitor

Every two weeks on Monday at 7 AM, checks NHS procurement portals for social prescribing contracts involving incumbent vendors. AI cross-references against known competitors. Contract expiring within 90 days? Decision-maker enrichment + "Competitor Displacement" CRM opportunity + Slack alert. Longer-dated contracts are logged for monitoring.

---

### WF-08: Leadership Appointment Monitor

Every Friday at 9 AM, watches for new Clinical Directors and Digital Leads at ICBs and PCNs. Enriches the contact. Then checks: does the parent healthtech company already operate in that geography? If yes: Slack alert for a warm intro opportunity. If no: add to cold outreach. Either way: CRM contact created with a "New Appointment" tag.

---

## How the Whole System Works Together

**Finding leads.** Four signal monitors (WF-01a, WF-04, WF-07, WF-08) scan the NHS landscape automatically — job vacancies, board papers, competitor contracts, leadership changes. Two engagement pipelines (WF-02a/b and WF-06a/b) capture people who interact with LinkedIn content. All of these feed leads into enrichment, then into the CRM and email campaigns.

**Working leads.** Once leads are receiving cold emails, WF-03 handles every reply with AI. Interested leads become deals. Objections get manual attention. "Not now" leads get re-engaged later. Wrong-person replies get automatically rerouted to the right decision-maker.

**Closing deals.** When a deal progresses to a signed pilot, the pilot lifecycle suite (WF-05a through WF-05d) takes over — onboarding, weekly health checks, milestone-triggered actions, and a case study generated from the pilot's results.

**Staying safe.** WF-00 watches everything. If anything breaks, the team knows immediately.

**The result:** the team's only manual work is responding to interested replies, running demos, and managing pilot relationships. Everything else happens automatically.

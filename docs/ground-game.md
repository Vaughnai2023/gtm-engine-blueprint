# The Ground Game

Domain warmup takes 2-3 weeks before cold infrastructure is ready to send. This document covers what to do in that window. Three tracks run in parallel: manual outreach to generate early conversations and demos, system setup to prepare the Claude Code workspace and CRM, and infrastructure warmup to get domains and inboxes ready for scale.

---

## Three Parallel Tracks

| Track | Timeline | Purpose |
|---|---|---|
| Manual outreach | Day 1 onwards | Generate conversations, validate ICP, test the offer |
| System setup | Days 1-14 | Claude Code workspace, CRM, scoring, copy frameworks |
| Infrastructure warmup | Days 1-21 | Domains, inboxes, SPF/DKIM/DMARC, warmup tools |

These run simultaneously. By Week 3, the infrastructure is warm, the system is built, and the ICP has been validated through real conversations. Cold outreach begins with tested messaging and a scored target list.

---

## Track 1: Manual Outreach (Day 1 Onwards)

### Working the Warm List

The parent company has 1,200+ GP surgeries, 18M patients, and roughly 15% GP market share. Over a million patients have been referred into 30,000 local services. Outcomes data shows 39% fewer GP visits, 26% less A&E usage, and 41% fewer fit notes when patients attend these services. This existing customer base provides a direct path to warm introductions at ICB level.

**Days 1-3: Map the network.**

1. Get the full customer list from the parent company
2. Map which ICBs those GP surgeries sit within
3. Rank the 10-15 ICBs with the highest existing GP coverage
4. Pull decision-maker titles at those ICBs: Director of Primary Care, Head of Social Prescribing, Digital Transformation Lead
5. Check whether the parent company has G-Cloud or DOS framework agreements the AI product can ride

**Days 3-7: Request introductions.**

Contact existing GP practice managers by phone. Phone calls convert better for introduction requests since the ask is immediate and harder to defer than an email.

The ask:

```
"Your practices already use our platform. We've built an AI link worker
that sits on top. Early results are strong. Can you connect me with
[Name] at [ICB]? I'd like to show them what it does."
```

**Days 7-14: Book demos from warm intros.**

| Approach | Expected Conversion to Demo |
|---|---|
| Warm intro from existing GP contact | 40-60% |
| Cold email to the same ICB | 5-8% |
| Cold email to an unknown ICB | 2-4% |

These early demos serve two purposes: they generate pipeline, and they produce the objection data and buyer language that cold outreach needs to be effective.

### LinkedIn

Connect with 15-20 ICP titles per day from Day 1. Engage with their content before sending DMs. Keep DMs under 50 words with a single ask:

```
Saw your post on the link worker recruitment challenge.
We've been working on something in this space with a few ICBs.
Worth a quick chat to see if it's relevant?
```

### Manual Emails (Personal Account)

While cold infrastructure warms, send 5-10 targeted emails per day from your personal work account to high-value contacts. These follow the same constraints as cold outreach:

- Under 50 words
- Plain text, no HTML
- No links
- Single ask
- Personalised first line referencing something specific to the recipient

```
Subject: quick question

[Name], a few GP practices in [ICB area] already use our social
prescribing platform. We've deployed AI link workers on top.

Early data: 39% fewer GP visits, 26% less A&E.
No recruitment, no waitlists.

Worth 15 minutes?
```

### First 10 Conversations

The ICP definition in [strategy.md](strategy.md) is a hypothesis. The first 10 conversations validate or correct it. After those calls, you should be able to identify:

- Which titles have both the pain and the budget authority
- The 3-4 objections that come up consistently (these become "without" statements in cold copy)
- How buyers describe the problem in their own words (this language goes directly into email sequences)
- Whether the current offer resonates or needs adjustment

---

## Track 2: Build the System (Days 1-14)

### Claude Code GTM Workspace

Set up the workspace following methodology. The structure is one folder per business with four critical reference files:

```
gtm-workspace/
├── CLAUDE.md              # Master directives
├── scoring-criteria.md    # ICP scoring rules
├── copy-frameworks.md     # Winning email templates by campaign type
├── data/                  # Company CSVs, contact lists, enrichment outputs
├── campaigns/             # Campaign logs and performance data
└── context/               # Case studies, proof points, competitive intel
```

CLAUDE.md contains four references:
1. Path to scoring criteria file
2. Contact data API (Apollo, Clay, or equivalent)
3. Path to copy frameworks file
4. Sequencer API (Instantly)

For each API, provide Claude Code with the API docs URL. It reads the documentation and creates its own reference files, then accepts the API keys. From that point, natural language commands work: "Score these 200 companies against our ICP." "Find Directors of Primary Care at Tier 1 ICBs." "Create a 3-email sequence using the workforce gap angle."

### Score Before Sending

Build target lists from data, not intuition.

1. Identify all companies within the TAM (42 ICBs, ~1,300 PCNs in England)
2. Score using the [5-dimension framework](scoring-criteria.md): market signals, firmographic fit, engagement history, competitive landscape, timing
3. Tier them: T1 (perfect fit), T2 (good fit), T3 (broad), Disqualified
4. Build targeted lists from the scoring output

Use Claude Code to write Python scripts for deterministic scoring against hard data: revenue, headcount, vacancy count, board paper mentions. Deterministic scoring avoids the inconsistency of LLM-based evaluation at this step.

### CRM Pipeline

Configure pipeline stages to match the NHS buying cycle:

1. Signal Identified
2. Outreach Sent
3. Reply Received
4. Demo Booked
5. Demo Completed
6. Internal Socialisation (this stage typically takes 2-6 months in NHS)
7. Pilot Proposed
8. Pilot Signed
9. Pilot Active
10. Expansion Opportunity

### Developing Copy Frameworks

The process for building copy frameworks that scale:

1. Pick a single real contact from the target list
2. Research them manually. Write a fully personalised email as if sending it yourself.
3. Identify what data made the email effective: company size, vacancy count, board paper mention, recent appointment
4. Replace the specifics with personalisation tokens. Structure into reusable sections (hook, body, CTA).
5. Save as a framework in `copy-frameworks.md` with token mappings and data requirements
6. Scale via Claude Code, which reads the framework and generates personalised emails for each contact using the required data

---

## Track 3: Infrastructure Warmup (Days 1-21)

### Domain and Inbox Setup

Purchase domains on Day 1. Warmup needs 2-3 weeks before any cold sending can begin.

| Scale | Active Domains | Inboxes per Domain | Sends per Inbox/Day | Daily Volume |
|---|---|---|---|---|
| Weeks 3-4 (start) | 2-3 | 2 | 10-15 | 40-90 |
| Month 2 | 4-5 | 2 | 10-15 | 80-150 |
| Month 3 | 6-8 | 2 | 10-15 | 120-240 |

These are active sending inboxes. The same number should be resting or warming at any given time. Total domain inventory is double the active count.

### Day 1 Checklist

- [ ] Domains purchased (4-6 minimum for Set A + Set B)
- [ ] 2 inboxes created per domain
- [ ] SPF, DKIM, DMARC configured on every domain
- [ ] Warmup started in Instantly (or equivalent)
- [ ] Custom tracking domain configured
- [ ] No cold emails sent until warmup has run for minimum 2 weeks

### The Set A / Set B Rotation

1. Label half the domains **Set A**, half **Set B**
2. Set A goes active first when warmup completes
3. Set A sends for 3-4 weeks. Set B continues warming or rests.
4. At the start of week 4, gradually ramp Set A down and Set B up
5. Repeat every 3-4 weeks

This prevents the inbox degradation that occurs around the 4-week mark. By the time Set A would have started losing deliverability, Set B is already warmed and taking over. Set A recovers during its rest cycle.

---

## Cold Email Infrastructure: Operational Detail

### Campaign Averages Hide Inbox-Level Problems

Campaign-level metrics (aggregate open rate, aggregate reply rate) can mask degradation at the individual inbox level. One inbox may be landing in spam while another performs well. The campaign average looks acceptable, but a significant portion of emails are not reaching the primary inbox.

I encountered this on a campaign that was performing well before results started sliding by a few tenths of a percent per day. The decline was gradual enough that it didn't trigger immediate concern.

The issue was not data or copy. I built an automation using the Instantly API to pull inbox-level performance data, broken down by batch (we run A/B inbox rotations on a 3-4 week cycle), and compared week-over-week trends.

The root cause: during the rotation handoff, the batches were swapped. Fatigued inboxes started ramping back up instead of down. Fresh inboxes were pulled back into rest. Once corrected, results recovered the same day and actually improved beyond the previous baseline.

This type of issue is invisible at the campaign level. It only surfaces when monitoring individual inbox performance over time.

### Per-Inbox Monitoring

Track these metrics for each inbox individually:

- Open rate (pull from active sending if it drops below 30%)
- Reply rate (week-over-week trend matters more than the absolute number)
- Bounce rate (spikes indicate data quality issues or domain flagging)
- Warmup health score
- Spam placement rate (where available)

### The Week 4 Degradation Pattern

A common failure mode: campaigns launch, early results look good, then around week 4 inboxes degrade. Reply rates can decrease up to 4x within two weeks on the same copy, data, and offer. The only variable that changed is inbox health.

The typical response is to rewrite copy or change the offer. This is usually the wrong diagnosis. If results were working in weeks 2-3 and declined in week 4, the infrastructure degraded, not the messaging.

The A/B rotation system described above prevents this. Without rotation, this degradation pattern is predictable and largely unavoidable.

### Infrastructure vs. Offer Problems

If reply rate is below 2% after testing 4-5 angles across clean, recently-warmed infrastructure, the issue is the offer, not deliverability. At that point, additional infrastructure investment will not improve results. The correct response is to return to conversations (Track 1), gather more objection data, and refine the offer.

The formula is Volume x Deliverability x Offer. Infrastructure handles the first two terms. If both are healthy and results are still poor, the offer is the remaining variable. When the offer underperforms, inbox degradation also accelerates beyond the normal 4-week cycle, since low engagement signals compound deliverability problems.

See the [copy framework](copy-framework.md) for the methodology on writing sequences and testing angles.

---

## Scaling

Initial setup and early sending is operationally straightforward. The complexity increases with volume.

At 50+ emails per day across 6-8 active inboxes, monitoring and rotating infrastructure takes 1-2 hours daily. At 100+ per day, infrastructure management approaches half-day effort. At that volume, the CRM integration, data pipelines, deliverability monitoring, and inbox rotation collectively require dedicated attention.

| Volume | Recommendation |
|---|---|
| Under 50/day, fewer than 5 active inboxes | Self-managed |
| 50-100/day, healthy reply rates | Consider a VA for inbox monitoring |
| 100+/day, rotation cycles every 3 weeks | Dedicated infrastructure person or outsource |
| Below 2% reply rate despite clean infrastructure | Pause scaling and fix the offer |

The initial manual test period (Weeks 3-6) serves to establish whether cold email produces results worth scaling. If the channel proves profitable at low volume, the infrastructure and processes are already in place for someone else to scale it.

---

## Verification Checkpoints

**End of Week 1:**

- [ ] 5+ conversations (calls) with prospects or existing customers completed
- [ ] Customer list mapped to ICBs, top 10-15 ICBs ranked by GP coverage
- [ ] 3+ warm intro requests made by phone
- [ ] Domains purchased, SPF/DKIM/DMARC configured, warmup running
- [ ] Claude Code workspace created with CLAUDE.md, scoring criteria, copy frameworks
- [ ] 50+ LinkedIn connection requests sent to ICP titles
- [ ] CRM pipeline stages configured

**End of Week 2:**

- [ ] 5-8 warm introductions initiated
- [ ] 2+ demo conversations booked
- [ ] Top 3 objections documented in the prospect's own words
- [ ] Target list scored and tiered (T1/T2/T3/Disqualified)
- [ ] First cold email sequences drafted (not sent, infrastructure still warming)
- [ ] Copy frameworks file populated with at least 2 reverse-engineered templates
- [ ] ICP scoring updated based on conversation data

**End of Week 4:**

- [ ] First cold campaigns live, open rates above 45%
- [ ] Inbox-level monitoring active (not relying on campaign-level dashboards)
- [ ] At least one underperforming campaign angle killed
- [ ] A/B inbox rotation plan documented and scheduled
- [ ] 4+ parallel campaign angles running (workforce gap, cost, outcomes, mandate)
- [ ] At least one warm demo completed with feedback on the offer

---

## What Comes Next

- Full 30/60/90 execution roadmap: [strategy.md](strategy.md)
- Automation that takes over from manual processes: [automation-architecture.md](automation-architecture.md)
- Email writing methodology: [copy-framework.md](copy-framework.md)
- Running the first pilot: [pilot-playbook.md](pilot-playbook.md)
- Lead scoring and routing: [scoring-criteria.md](scoring-criteria.md)

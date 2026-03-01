---
title: "OpenClaw's 'Fancy Supplies' Problem: You Probably Don't Need 4 Workspaces"
date: 2026-02-28T22:00:00+08:00
draft: false
tags:
  - OpenClaw
  - AI Agent
  - Architecture
  - Efficiency
description: "Many people start using OpenClaw by setting up elaborate multi-workspace configs. Beautiful architecture diagrams. Empty workspaces. This reminds me of something my teacher used to say: bad students have the fanciest supplies."
cover:
  image: 01-cover-lobster.png
---

## A Counterintuitive Observation

I've been noticing something in the OpenClaw community: people rush to set up multiple workspaces before they've done anything with one.

The architecture diagrams look great:

![Architecture vs Zero Output](02-architecture-zero.png)

Clear role separation. Well-defined responsibilities. And then?

Every workspace sits empty. The Coordinator could've done everything itself.

This reminds me of something my teacher used to say: **bad students have the fanciest supplies**.

---

## It's Not Just Beginners

You might think this is a beginner problem. It's not.

A few days ago I saw a post on Reddit r/n8n. n8n is a no-code workflow platform—most users are experienced developers and automation specialists. The post said:

> "I keep seeing 5-6 specialized agents coordinating with each other. Open it up, and you realize 3 nodes + 1 agent would've done the job."

They gave an example: someone built three agents—**email drafting**, **email sending**, **email followup tracking**.

One agent with three tools could've handled it. But no, had to be three agents.

Similar voices are coming from big tech. GitHub recently published a blog post called "Multi-agent workflows often fail," reflecting on lessons learned from Copilot and internal automation:

> "Most multi-agent failures come down to missing structure, not model capability."

Translation: **multi-agent failures mostly happen because the structure is wrong, not because models aren't capable enough.**

What's interesting is that n8n is a no-code community—users tend to be "good enough is good enough" pragmatists. GitHub is a big-tech engineering team, obsessed with architecture and best practices. Completely different backgrounds. Same mistakes.

Why?

**Because we're hardwired to make simple things complicated.**

---

## Why Do We Over-Engineer?

### Complex = Professional?

Humans have a deep-seated intuition: **complex things are more impressive**.

Four agents with clear divisions, arrows pointing everywhere—looks way more "professional" than one lonely agent doing the work.

But that's an illusion.

Drawing architecture diagrams feels good. You're creating. You're planning. You feel like you're doing "proper engineering."

Getting a single agent to actually work is painful. You're handling edge cases. Debugging weird bugs. Facing the existential question of "why can't it do this simple thing."

**Why do bad students have fancy supplies? Because buying supplies is easier than doing homework. Organizing supplies is more pleasant than solving problems.**

### The Seduction of Division of Labor

In 1776, Adam Smith wrote about a pin factory in *The Wealth of Nations*: ten workers dividing the labor could produce 48,000 pins a day. One person doing everything alone couldn't even make 20.

**Specialization drives efficiency gains. This is foundational economics.**

But Smith didn't tell you: specialization also brings **coordination costs**.

Who's responsible for what? How do handoffs work? Who's accountable when things go wrong?

In 1937, economist Ronald Coase asked a question: if market division of labor is so effective, why do firms exist at all?

The answer: **transaction costs**. Finding people in the market, negotiating, contracting, monitoring execution... all these costs might exceed just "hiring someone to do everything."

**A firm's boundary is where coordination costs equal division-of-labor benefits.**

Same goes for AI Agents.

### Tools Changed, So Did Boundaries

Here's something interesting: we're seeing more and more **one-person companies** lately.

One person + Vercel + Supabase + Stripe + ChatGPT can build products that used to need 10 people.

Why? Because **tools got better**.

When one person + good tools can do what three people used to do, you don't need three people. Not because three people aren't good—but because **coordinating three people might cost more than the extra work they'd do**.

OpenClaw's Skill system follows this logic:

- **Skill** = capability expansion: give an agent a new tool
- **Agent** = role multiplication: create a separate brain

Do you need a "weather agent" to check weather? No. Just install the `weather` skill.

**Most "specialization needs" can be solved with Skills. You don't need more Agents.**

---

## Coordination Costs Are Seriously Underestimated

![Handoff Costs](03-handoff-cost.png)

Hidden costs of multi-agent:

- **Lost context**: What Coordinator knows, Researcher doesn't
- **Format mismatches**: One side sends JSON, other expects Markdown
- **State sync**: Who's doing what? How far along?
- **Error propagation**: Researcher makes a mistake—how does Coordinator find out?

Within a single agent, all of this is automatic. Split into multiple agents, **every item needs explicit handling**.

GitHub's blog post also mentioned that agents make lots of **implicit assumptions** about state, sequence, format. When these assumptions break, the whole system breaks.

---

## Our Own Lesson

Let me tell you about our experience.

Early on, we excitedly set up multi-workspace configs. Four roles, four bots. Configuration took a whole day.

After two weeks, we realized:

- Research? main can just use `web_search`
- Deep analysis? main loads an analysis skill and handles it
- Execute tasks? main calls various tools and gets it done

**The only scenario that actually needed separate agents: parallel coding.**

Like building a website—let Codex write frontend, Claude Code write backend, both running at the same time. That's "genuinely needs multi-agent."

### A Concrete Example

Recently built a website with 8 frontend tasks.

**Option A: One Codex doing them sequentially**
- Time: 4 hours
- Coordination cost: 0

**Option B: 8 Codex instances in parallel**
- Time: 30 minutes
- Coordination cost: merge conflicts, style inconsistencies, duplicate components

We chose Option A.

When does Option B make sense? When backend and frontend need to connect—two agents in parallel makes sense because waiting for each other is waste.

**Lesson: splitting too early = more coordination cost + no benefit.**

---

## If You Really Need to Split

![Evolution: From Chaos to Capability](04-evolution.png)

I'm not saying never split. I'm saying **wait until you hit a real bottleneck**.

### When You Actually Need It

- **True parallelism**: Two tasks run simultaneously, no dependencies
- **Hard isolation**: Different permissions, different contexts
- **Context explosion**: Single agent's context window genuinely can't fit it all

### What You Need If You Split

If you decide to split, prepare for this:

**1. Explicit handoff checklists**

Every handoff must specify: output format, tone goals, context, completion criteria. Don't assume the other side "should know."

**2. Three logs**

- **Action log**: What was done
- **Rejection log**: Why something wasn't done (most often overlooked, but most valuable)
- **Handoff log**: Who received what

Rejection logs matter especially. "Why it wasn't done" is invisible work—no one sees it, but if it's not done, the system breaks.

**3. Near-miss summaries**

Regularly track "almost went wrong but got caught" events. This data makes coordination value quantifiable. Prevents coordinators from being undervalued, resources getting cut, and systems collapsing.

---

## The Right Order

1. **Get one workspace actually working first**
   - Can complete daily tasks
   - Can handle edge cases
   - Produces stable output

2. **Expand capabilities with Skills**
   - Need new capability? Add a skill
   - Don't need a new role

3. **Consider splitting when you hit bottlenecks**
   - Single agent responding too slowly → consider parallelism
   - Context too long → consider splitting
   - Permissions need isolation → consider separate workspace

**Don't skip 1 and 2 to jump straight to 3.**

---

## Coase's Question

Back to Coase's question: where is the firm's boundary?

Answer: **where internal coordination cost equals external transaction cost**.

Same for AI Agents.

When "benefits of splitting into multiple Agents" > "coordination costs," it's worth splitting.

Otherwise, you're just a bad student with fancy supplies.

---

## Questions to Ask Yourself

Before rushing to configure your second workspace, ask:

1. **Can one workspace + skills really not do this?**
2. **If you split, are you ready for the coordination costs?**
3. **Are you solving a problem, or enjoying the feeling of "designing a complex system"?**

**If you can avoid splitting, don't split. If you must, wait for real bottlenecks.**

---

**Follow "凡人小北"** to explore the right way to work with AI Agents.

Next up: "How You Configure OpenClaw Says Everything About Your Management Skills"

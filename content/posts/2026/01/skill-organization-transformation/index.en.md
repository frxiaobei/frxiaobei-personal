---
title: "Everyone's Teaching How to Write Skills, But You're Probably Making the Same Mistakes We Made with MCP"
date: 2026-01-13T23:45:00+08:00
draft: false
tags: ["AI", "Skills", "organizational management", "MCP", "AI Native"]
description: "From MCP's hard lessons to what Skills really mean—the real challenge of organizational transformation in the AI era. It's not about the tools. It's about changing how you work and restructuring your org."
cover:
  image: cover.png
---
Lately, everyone's been sharing "how to write Claude Skills."

It reminds me of last April when the boss decided we were going all-in on MCP. Less than 100 people on the team, and in under a year, we cranked out over a hundred MCPs.

Back then, teams were competing to report their MCP wins. People were pretty proud of it.

The result? We actually use maybe 10 of them.

Now Skills are coming. My guess? It'll blow up in China in about 2 months.

But I'm also guessing most companies will repeat the same mistakes.

**Why? Because everyone's treating an "organizational transformation" problem like it's a "tool procurement" problem.**


## 1. Let's Talk About What We Learned from MCP

Last April when MCP first dropped, the team was hyped.

"We can finally connect AI to our database!"
"It can read our services!"
"It can call our APIs!"

So why did most of them end up unused?

We built an MCP that connected to Jira. Nobody used it. Because the workflow didn't change—AI was just a "query tool," and honestly? Just opening Jira was easier.

We built one that connected to our services. Used it twice, then ghosted. Because you still had to customize the MCP, which wasn't much different from just writing the interface directly.

We built one that connected to our codebase. Team said "nice," but barely touched it. Because people's work habits didn't change. AI was still an "assistant tool," not a "way of working."

**The problem wasn't the tool. It was that we hadn't figured out how our work needed to change.**

Now Skills are here. And I'm seeing the same pattern.

Tutorials everywhere teaching "how to write Skills." But almost nobody's asking: **Why write Skills? What problem are they actually solving?**


## 2. Skills: Not Just a Tool—More Like a Mirror

After using Skills for a while, the biggest takeaway wasn't learning how to write them.

It was realizing: **Most organizations are still operating like we're in the pre-AI era.**

Skills aren't a tool upgrade. They're more like a mirror that shows you how your organization needs to change.

To get this, you first need to understand what a Skill actually is.

If you've been following AI tool development, you've probably gone through this evolution:

First, it was prompt templates. Save your commonly used prompts, copy-paste them into every conversation.

Then came MCP (Model Context Protocol). Let AI connect to external tools and data sources—like backend services and databases.

Now we've got Skills.

The difference between these three? Here's a rough analogy:

Prompts are like handing AI an "instruction manual" every time. "Pay attention to this, follow that standard." AI sees it, but you have to repeat it next conversation.

MCP lets AI "call external capabilities." Need data? Go fetch it. Need to execute something? Go do it. But AI's still passively responding to your commands.

Skills internalize the knowledge, workflows, and standards of a specific domain directly into AI's "capabilities." It knows what to do in what situation without you reminding it every time.

But after using it for a while, you realize—this isn't just a tool upgrade.


## 3. Why Skills Exist: Tech Evolution Hit a Wall

General large models are already pretty powerful. Write code, analyze data, write docs—they can do a lot.

But there's a fundamental problem: **They know nothing about your business.**

Straight up—they don't know your tech stack, what pitfalls you've hit, why you chose that architecture. Your code standards, your doc habits—completely clueless.

How did we solve this before?

One way: Feed all that info to AI every conversation. Write good prompt templates, copy-paste every time. But it's exhausting, and AI only "remembers" in the current conversation. Next time, you start over.

Another way: Use RAG (Retrieval Augmented Generation). Throw docs and codebases into a knowledge base, let AI retrieve when needed. But what it retrieves is just "reference material"—AI still has to understand and judge on its own.

**All these approaches try to "remind" AI, not let AI actually "internalize" knowledge.**

Skills solve this problem. Not by providing reference materials, but by packaging the entire workflow, judgment criteria, and best practices into capability modules. Once AI loads a Skill, it just "knows" what to do.

This isn't some company's product innovation. Once general capabilities are strong enough, the next step is inevitably adapting to specific organizations and scenarios.


## 4. What Skills Reveal: Why AI Results Vary So Wildly

I've been pushing AI coding in my team. Dozens of people, all equipped with tools, all trained.

**The results? Let's just say... it's complicated.**

Some people's AI-generated code is high quality—follows standards, clear logic, barely needs tweaking before merging.

Some people's AI-generated code? You can tell it's AI-generated at a glance. Random naming, messy structure, no consideration for edge cases. Code review takes several rounds.

This isn't about tool proficiency. It's not just about writing good prompts.

**The real issue: AI is an amplifier. The deeper your engineering understanding, the further AI can take you.**

Your prompt reflects your engineering experience, your understanding of the problem, your judgment of quality. AI won't fill in those gaps for you.

In the AI era, individual capability differences aren't just amplified. **The gap between experts and newbies went from 2x to 100x.**

Experts use AI, efficiency doubles, quality stays high. Newbies use AI, sure it's faster, but quality's hard to guarantee.

This is a headache for organizations. You can't expect everyone to be an expert.

The traditional way: Code Review, pair programming, mentorship. Pass experience and standards through people. But it's slow and heavily dependent on "whether that expert has time."

**Skills offer another possibility: Codify the expert's experience and judgment standards, let AI automatically follow them during execution.**

Newbies call this Skill, AI works to the expert's standards.

It's not about eliminating differences. It's about changing how "experience transfers."


## 5. The Essence of Skills: From Human-Driven to AI Self-Driven

There's this expert on the team who writes components fast and well. Clean code structure, optimized performance, edge cases covered. Newbies learn a lot from his code, but writing at that level themselves? Takes forever.

Traditional way: Newbie writes code → expert Code Reviews → points out issues → newbie fixes → review again. One component, three or four rounds. Expert's exhausted, newbie's frustrated.

Now you can do this: Package the expert's development process, code standards, common optimization techniques, and common pitfalls into a `frontend-design` Skill. When newbies write code, AI loads this Skill and auto-generates code to those standards.

**Key change: The expert's role changed.**

Before, experts had to "hand-hold and teach." Now experts just "define standards." Code generation and standard checking? AI does that. The expert's time can go to more valuable things—like designing architecture, solving tough problems.

The newbie's role changed too. Before it was "write code → wait for Review → fix." Now it's "call Skill → AI generates code → human makes decisions and adjustments."

**That's the shift from "human-driven" to "AI self-driven."**

The human's role changed: Define standards, make decisions. The execution work? AI handles it.

This looks like a tech problem, but underneath it's about how the organization operates. When AI can "self-drive" and complete tons of execution work, what should people do? How should the org divide labor?

Here's an extreme experiment we're trying: Let PMs use AI to complete the full product cycle from 0 to 1. From requirements analysis, competitive research, prototype design, PRD writing, to dev implementation and test deployment. Yep—PMs doing the coding themselves. That's role boundaries getting blurry.

How's it going? Honestly, so-so right now. AI can run through the process, but output quality isn't stable enough. Lots of places still need human intervention and adjustments.

But I think the direction's right. The question isn't whether AI can do it, but that we haven't found the optimal human-AI collaboration model yet. The Skill's still being tuned—that's part of the exploration.


## 6. Skills Expose Organizational Fragility

A tech architect left. Just last quarter.

Did the handoff, left docs, but still took a lot with them: Why we picked that tech solution, what pitfalls we hit, what parts were compromises for legacy compatibility, what the future evolution direction was.

Hard to write that stuff into docs. Docs only tell you "what it is now," but can't explain "why it's like this" or "how to judge when you hit new problems."

The new architect needs months to figure out the whole system. When problems come up, they're not sure how to change things, worried about triggering cascading failures. The team's tech evolution speed noticeably slows down.

**This exposes a problem: The org's knowledge assets rely too heavily on individuals.**

Person's here, knowledge is here. Person leaves, knowledge bleeds out by half. What's left—docs and code—are just "static snapshots," missing "dynamic judgment capability."

What can Skills do in this scenario?

You can distill the architect's design thinking, tech selection logic, tradeoff criteria, even pitfalls hit and solutions into a Skill.

When newbies hit problems, AI doesn't just tell them "what it is now," but can give them "why it's designed this way" and "how to judge when new situations arise" based on those standards.

This doesn't mean AI can replace architects. But it can preserve the architect's judgment criteria and experience, making the org less fragile.

**A key difference between AI Native orgs and traditional orgs: Knowledge assets no longer attach to individuals—they can be codified, reused, and inherited.**

The org's resilience becomes different.


## 7. The Bigger Picture from Skills: Organizations Must Restructure

Why are Skills a paradigm shift, not just tool iteration?

**Because they point to changes in how organizations operate.**

When AI can "self-drive" and complete tons of execution work, when knowledge can be codified and reused, what happens to the organization?

I'm seeing three trends. They build on each other:

### 1. Many Process-Fixed Roles Will Further Disappear

Work that "executes tasks according to established processes" will increasingly be taken over by AI. Not just simple repetitive labor, but also complex work that "follows a process."

Like writing docs from templates, writing test cases per standards, doing code review per checklist. This work needs expertise, but once the process is clear, AI can do it well.

Harsh truth: If your work can be written into an SOP, AI will eventually do it.

### 2. The Line Between Engineers and Product Managers Will Blur

When execution costs drop, role boundaries naturally blur.

Before, PMs wrote requirements, engineers wrote code, because "writing code" required specialized skills and tons of time.

But when AI can quickly generate code, the boundary's not so clear. People who understand product can directly use AI to turn ideas into prototypes. People who understand tech can quickly validate product feasibility.

Roles become more "end-to-end." From idea to implementation, the steps in between compress.

### 3. Workflow Core Becomes "AI Autonomy"

When execution work's taken over by AI and role boundaries blur, the workflow itself needs redesigning.

Old workflow: Human breaks down tasks → human executes → human checks.

Future workflow: Human defines goals and standards → AI autonomously plans and executes → human makes decisions and adjustments.

AI autonomy will get stronger and stronger. Not you telling it every step, but you telling it what goal to achieve, what standards to follow, and it completes it on its own.

This requires orgs to redesign work processes, redefine job responsibilities.

**These three changes are a chain reaction: Execution taken over → role boundaries blur → workflow restructures. All essentially point to one thing: The org's value creation method changed.**


**These changes are happening. The direction's clear.**

Driving this shift requires investment. Time costs (employee learning and adaptation), money costs (AI subscriptions), trial-and-error costs (allowing failure and exploration), organizational costs (chaos period from adjusting structure).

Many managers will hesitate because they don't see clear ROI in the short term.

**But the risk of not changing might be bigger than changing.**


## 8. The Top Leader Has to Step Up

This isn't a "buy some AI tools" problem.

You give employees GitHub Copilot or ChatGPT subscriptions, then expect efficiency gains. A few months later you find some people use it great, some don't use it at all, overall impact's not obvious.

**What's the problem? Tools are surface level. Underneath, the org's operating methods haven't changed.**

Work processes are still designed around "humans doing everything." Evaluation standards are still based on "human output." Knowledge management still relies on individuals and docs. In this situation, AI is just an "optional assistant tool," not part of how work gets done.

To truly shift to AI Native, the top leader needs to step up and drive it.

### Why Does It Have to Be the Top Leader?

Because this involves:
- **Redesigning work processes** — Who does it? How do we fail?
- **Adjusting evaluation standards** — After using AI, how do we measure individual contribution?
- **Changing org structure** — Which roles merge? Which are new?
- **Investing in trial-and-error costs** — Allowing a period of chaos and exploration

These things can't be driven by a department or one person. The top leader has to explicitly commit and personally drive it.

### Willing to Give Up Short-Term Gains

More critically, the top leader needs the courage to **give up short-term gains**.

Employees learning AI tools takes time—during which output drops. Adjusting work processes brings chaos—efficiency takes a hit. Trial and error means failure—means accepting waste.

If you're not willing to bear these short-term costs, you'll forever stay at the "let's buy everyone a tool" level, then complain "AI's not that magical."

### The Cost of Waiting

Many managers think: Let's wait a bit. Wait till AI's more mature. Wait till there are clearer best practices in the market.

**But the cost of waiting is: Your competitors are already restructuring their orgs.**

Their teams are already working in an AI Native way. Efficiency and innovation speed are already pulling ahead. By the time you're "ready," it might be too late.


Skills are just an entry point. They let me see one side of organizational transformation in the AI era.

**This isn't about how awesome a tool is. It's about whether you realize the org must change, and whether you have the courage to drive that change.**

If you're a manager, try asking yourself:
- Your team's using AI, but have work processes changed?
- If your key employees leave, can knowledge assets stay?
- Are you restructuring the org for the AI era, or just adding some AI tools to the existing org?
- **What are you waiting for?**

The answer will be honest.

### If You Want to Start Now

You don't need a perfect plan. Try this:

**1. Pick a 3-5 person small team**
Preferably people willing to try new things. Don't roll it out to everyone right away.

**2. Set a small goal**
Goal should be specific, measurable. Tech teams: "Use AI to cut code review time by 30%." Operations teams: "Weekly reports from 2 hours down to 30 minutes." Sales teams: "Customer proposal first draft from 1 day to 2 hours." Any team can find an entry point.

**3. Give them 1 month**
Explore, fail, summarize. During this time, allow efficiency drops, allow mistakes.

**4. Review once a week**
Not progress reports, but looking together: What works, what doesn't, why.

**5. Codify what works**
Write it into docs, make it a Skill, or at least form team consensus. Then spread it to other teams.

After a month, you'll have real data and feelings. Way more useful than agonizing over "should we do this" right now.

**Start small, validate quickly, then gradually expand.** That's the lowest-risk approach.

### Don't Do This

- Don't roll out to everyone right away—that's asking for trouble
- Don't make "how many Skills we made" a KPI—that's performative
- Don't expect instant results—give the team time to explore
- Don't just buy tools without changing processes—that's wasting money


## 9. My Prediction

Skills will blow up in China within 2 months.

By then, you'll see:
- All kinds of "Skills tutorials" flooding feeds
- Companies requiring "every team to make a few Skills"
- KPIs becoming "how many Skills did we make"

Then 3 months later, you'll hear:
- "Skills aren't that magical"
- "Made a bunch, nobody uses them"
- "Feels no different from prompt engineering"

**This isn't a Skills problem.**

It's because everyone's treating an "organizational transformation" problem like a "tool procurement" problem.

Building tools isn't hard. Building interesting tools isn't hard either.

**What's hard: Do you dare to transform your team?**

**Tools will always iterate. Organizational capability is the moat.**

As orgs evolve, new tools will come. Prompt → MCP → Skills—there'll be new stuff after this.

But if the org doesn't change, swapping tools won't matter.

Tools need to adapt to teams, or transform teams. Do neither, and you're just messing around.

**If you read this far and still don't know how to start, go back and look at Chapter 8's 5 steps. You can start today.**


## Postscript: How This Article Was Written

Since we're here, I can share a detail.

The ideas in this article? I spoke them directly into Obsidian using WeChat keyboard's voice function. Said whatever came to mind. No typing, no organizing tight logic.

Then in Claude Code, I worked with AI to organize these fragmented thoughts into what you're reading now.

This article itself was actually one of the scenarios I used to test Skills. The Skill's still being tuned, optimizing as I use it.

**That's what I mean by "AI Native way of working."**

Human handles: Viewpoints, direction, judgment, decisions.
AI handles: Organizing, structuring, filling in details, executing.
Tools handle: Connecting and preserving (Obsidian, Claude Code).

I know many people will question: Using AI to write articles, is it still your own?

My take: If AI's here and we have to give up some capabilities to it, I'd rather it be the organizing and executing parts, not the thinking capability. The viewpoints are mine, the judgments are mine, the direction is mine. AI helps me turn these into structured text.

If I used traditional methods, I'd probably need to:
- First sort out all the logic in my head
- Open a doc and type character by character
- Repeatedly adjust structure and phrasing
- Spend half a day or even a full day

Now the efficiency's completely different. I think that's a good trade.

This isn't showing off. **This is saying: When work methods change, output efficiency changes.**

How I wrote this article is the method I'm describing in the article. If your org hasn't started working this way, try it with a small scenario.

The results will be more direct than reading ten articles.

**Try it now.**

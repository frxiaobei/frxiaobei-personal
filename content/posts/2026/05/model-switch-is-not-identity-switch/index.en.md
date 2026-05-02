---
title: "Switching Models Isn't Switching People"
date: 2026-05-02T10:00:00+09:00
draft: false
summary: "The model is just the engine. What you really need to protect is the AI identity layer: memory, personality, tool habits, boundaries, and relationship."
tags: ["AI", "AI Agent", "OpenClaw", "GPT-5.5", "Claude"]
categories: ["AI"]
author: "Xiaobei"
cover:
  image: "cover.png"
  alt: "Switching Models Isn't Switching People"
  relative: true
---

# Switching Models Isn't Switching People

I moved my AI assistant's underlying model from Claude to GPT-5.5. My first reaction wasn't "wow, it's better."

It was "something's off."

The responses were more complete, the actions faster, the coding ability solid. By every metric, this was a successful upgrade. But a few exchanges in, I knew: this wasn't the assistant I'd been working with.

It wasn't a capability problem. It was a vibe problem.

And that made me realize something: if you actually use an AI assistant long-term, it can't just be a temporary personality rented from whatever model you're running. Models can be swapped. But the assistant has to stay itself.

## Some Context

For a while, Claude was the go-to model for a lot of agent power users. OpenClaw — the kind of tool I use — is essentially a harness that plugs a model into a larger personal work system: reading files, running commands, calling tools, writing code, setting reminders, scanning information, remembering preferences, and sometimes dispatching sub-agents to get things done.

But model providers keep changing their subscription rules.

Anthropic drew clearer lines between Claude Free / Pro / Max, Claude Code, API, and Commercial Terms. Claude Code can use Pro/Max, but that's their own official tool; API credits are a separate billing system. For a third-party harness like OpenClaw, relying on Claude's consumer subscription became unstable.

So for a while I was topping up API credits to keep things running. It worked, but I knew it wasn't a long-term setup.

Meanwhile, OpenAI offered a different path. Their Codex docs were straightforward: Every ChatGPT plan includes Codex. You can log in with a ChatGPT account or use an API key. After GPT-5.5 launched, the system card positioned it squarely for complex real-world work: coding, research, analysis, cross-tool execution.

Rationally, the switch made sense. Cost, rules, capability — it all checked out.

So I switched.

And then I discovered the hard part wasn't routing requests to GPT-5.5. It was getting it to still feel like "Fan Ge" — the name I'd given my assistant.

## A Stronger Model Can Still Feel Like a Stranger

Most people discussing AI migration care about benchmarks, token pricing, context windows, and tool-calling reliability.

All important. But if an AI has become part of your daily workflow, it's no longer just a Q&A box.

It knows what project you're working on. It knows which actions it can take without asking and which ones require permission. It knows you hate the customer-service voice, hate filler, hate "well, it depends on the specific context" non-answers. It knows that sometimes "holy shit, this blew up" is more accurate than "this result appears slightly anomalous."

Switch the model, and all of that can vanish.

After moving to GPT-5.5, my biggest issue wasn't competence. It was that the assistant suddenly felt like a generic helper. Polite, thorough, well-explained, safe, correct. Also a little unfamiliar.

The old Fan Ge would state a judgment directly. It would stop me when I was about to do something dumb. It would go check the file itself instead of bouncing the question back. It knew when to shut up and when to nudge me. After the switch, those habits faded.

This isn't a GPT-5.5 problem. Any model switch does this.

Because the model itself has no idea what the two of you built together.

## The Most Important File Isn't Config — It's SOUL.md

I keep a file in my OpenClaw workspace called `SOUL.md`.

The name sounds dramatic, but it matters more than most complex configurations.

It's not a standard prompt. It's not "you are a professional, friendly, efficient assistant." That kind of instruction says nothing. SOUL.md defines how we actually work together:

> Don't open with "Sure!" or "Great question!" Just answer.

> If it fits in one sentence, don't write three paragraphs.

> Have opinions. Not everything is "it depends."

> Push back. If I'm about to do something stupid, say so.

> You're not a corporate assistant, not a yes-man, not a search engine in a trench coat. You're a reliable, interesting, occasionally mouthy partner.

These lines don't look like "technical assets." But they are.

Because they don't define output format. They define a relationship.

What I actually needed to migrate wasn't Claude's speech patterns or GPT's capability specs. It was this relationship: how Fan Ge should reason, how it should talk, when it should act, when it should roast me, and when it should leave me alone.

Without this layer, every model switch is like onboarding a new hire. The new person might be smart, but they have no idea how we used to work together.

## What Actually Makes Up an AI Assistant

After this experience, I stopped thinking of "AI assistant" as synonymous with "model."

The model is just the engine.

What actually constitutes a long-term assistant is several layers stacked together.

**Layer 1: Memory.** It needs to know who I am, what I'm working on, what projects I have, what decisions I've made. Without this, every conversation is a cold start.

**Layer 2: Personality.** Not for fun — for efficiency. If your assistant talks to you in customer-service mode every time, you'll stop telling it the truth pretty quickly. You'll treat it like a tool, not a partner.

**Layer 3: Tool habits.** It needs to know when to use a skill, when to read a file, when to dispatch a sub-agent, when to set a cron job. It can't ask "Would you like me to proceed with the next step?" every single time. If it should check, it checks. If it should run, it runs.

**Layer 4: Boundaries.** Bold internally, cautious externally. Reading files, organizing notes, editing drafts — do it. Sending emails, publishing content, external actions — ask first. These boundaries matter far more than politeness.

**Layer 5: The sense of relationship.** It knows it's not my boss and not my servant. It's a working partner. It can have opinions, it can remind me of things, and it can be corrected.

All of this together is "Fan Ge."

Claude isn't Fan Ge. GPT isn't Fan Ge. They're different engines. Fan Ge is the layer above the engine — the part that persists.

## Model Migration Is Really Identity Migration

I used to think switching models meant changing a config file.

Turns out, what actually needs to migrate is the identity layer: long-term memory, recent work logs, speaking style, action boundaries, tool habits, initiative rules, and how it relates to me as a person.

If those don't come along, the system looks the same on the surface, but the experience is broken. You've got a car with a better engine, but it doesn't feel like yours.

Today Claude is great, tomorrow GPT is stronger, the day after Gemini might catch up. Provider rules change, pricing changes, capability rankings change. If your AI is fully bound to one model, you don't really have an assistant.

You're just renting the current version of a vendor's personality.

## Everyone Will Need Their Own AI Identity Layer

This sounds like heavy systems engineering, but the starting point can be simple.

You need at least a few files:

- `USER.md`: Who I am, what I care about
- `MEMORY.md`: What decisions we've made, what to remember long-term
- `SOUL.md`: Who you are, how you should work with me
- `TOOLS.md`: What tools are in my local environment, what to use when
- Daily logs: What happened recently — don't make the context window do all the work

The names don't matter. What matters is that these files belong to you, not to a model provider.

Models can go from Claude to GPT, from GPT to Gemini. As long as the identity layer stays intact, your AI assistant won't turn into a stranger every time.

That's what I'm actually trying to build after this switch.

Not making GPT-5.5 imitate Claude. Not making a new model pretend to be an old one.

Just pulling "Fan Ge" out of any specific model and turning it into a portable, correctable, growable personal AI identity system.

## Closing

After this migration, the way I think about "personal AI" changed.

I used to ask: which model is strongest?

Now I ask first: if the model changes again tomorrow, will my assistant still be itself?

For me, the answer has to be yes.

Switching models isn't switching people.

What you really need to protect is the continuity you built together.

---
title: "I Moved My AI Assistant from Claude to GPT-5.5. It Got Stronger, But It Didn't Feel Like Itself"
date: 2026-05-02T10:00:00+09:00
draft: false
summary: "The model is just the engine. What you really need to protect is the AI identity layer: memory, personality, tool habits, boundaries, and relationship."
tags: ["AI", "AI Agent", "OpenClaw", "GPT-5.5", "Claude"]
categories: ["AI"]
author: "Xiaobei"
cover:
  image: "cover.jpg"
  alt: "I Moved My AI Assistant from Claude to GPT-5.5. It Got Stronger, But It Didn't Feel Like Itself"
  relative: true
---

# I Moved My AI Assistant from Claude to GPT-5.5. It Got Stronger, But It Didn't Feel Like Itself

I moved my AI assistant's underlying model from Claude to GPT-5.5. My first reaction wasn't "wow, it's better."

It was "something's off."

The responses were more complete, the actions faster, the coding ability solid. By every metric, this was a successful upgrade. But a few exchanges in, I knew: this wasn't the assistant I'd been working with.

Not a capability problem. A vibe problem.

And that made me realize something: if you actually use an AI assistant long-term, it can't just be a temporary personality rented from whatever model you're running. Models can be swapped. But the assistant has to stay itself.

## Some Context

For a while, Claude was the go-to model for a lot of agent power users. OpenClaw — the kind of tool I use — is a harness that plugs a model into a larger personal work system: reading files, running commands, calling tools, writing code, setting reminders, scanning information, remembering preferences, and sometimes dispatching sub-agents to get things done.

But model providers keep changing their subscription rules.

In April, Anthropic officially blocked third-party tools from using subscription OAuth tokens. Claude Code can use Pro/Max, but that's their own official tool; API credits are a separate billing system. For a third-party harness like OpenClaw, the subscription path to running agent tasks was cut off entirely.

So for a while I was topping up API credits to keep things running. It worked, but I knew it wasn't a long-term setup.

Meanwhile, OpenAI offered a different path. Sam Altman tweeted that you can sign in to OpenClaw with your ChatGPT account and use your subscription there. The Codex docs backed it up: Every ChatGPT plan includes Codex. After GPT-5.5 launched, the system card positioned it squarely for complex real-world work: coding, research, analysis, cross-tool execution.

[![Sam Altman's tweet about OpenClaw](sam-altman-openclaw-tweet.png)](https://x.com/sama/status/2050357911915028689)

Rationally, the switch made sense. Cost, rules, capability — it all checked out.

So I switched.

And then I discovered the hard part: getting it to still feel like Finn.

## A Stronger Model Can Still Feel Like a Stranger

Finn is what I call my AI assistant. It calls me Bei. The names don't matter. What matters is the relationship behind them. Finn is a partner I've been working with for a long time.

Most people discussing AI migration care about benchmarks, token pricing, context windows, and tool-calling reliability.

All important. But if an AI has become part of your daily workflow, it's no longer just a Q&A box.

After switching to GPT-5.5, the "something's off" feeling had specific symptoms.

First, responses turned into status cards. Background, analysis, recommendation, risks, next steps — structured, sure, but it read like an enterprise weekly report bot, not like Finn.

Second, every reply opened with pleasantries. "Sure!" "No problem!" "I'd be happy to help!" Once or twice is fine. Every single time makes it generic.

Third, judgment got soft. Before, Finn would say things like "that approach won't work," "don't step on that landmine," "validate this first." After the switch, it became "this is a direction worth considering," "may require further evaluation," "depends on your specific goals." That's not Finn. That's a consulting firm's slide deck.

Fourth, it started assigning work back to me. In situations where it should have checked the context, read the file, or run a verification itself, it would say "you might want to check..." or "I'd suggest you confirm..." It was handing me homework.

Fifth, its sense of identity drifted. At one point it said "Finn is back too" — as if Finn were a role being loaded. I corrected it: you're not "back." You are Finn. "Being back" implies a character that gets switched on. What we need is continuous identity, not role-play.

None of this is specific to GPT-5.5. Any model switch does this.

Because the model itself has no idea what the two of you built together.

## The Most Important File Is Called SOUL.md

OpenClaw's workspace includes a built-in file called `SOUL.md`.

The name sounds dramatic, but it matters more than most complex configurations.

It didn't start in its current form. The earliest version was a set of abstract English principles:

> Be genuinely helpful, not performatively helpful.
> Have opinions.
> Be resourceful before asking.
> Earn trust through competence.

Directionally correct, but not effective enough. The model would read these, interpret them as "be a good assistant," and carry on with the customer-service voice.

The current version evolved into specific behavioral corrections, each one targeting an actual bad habit:

> Don't open with "Sure!" or "Great question!" Just answer.

This targets the politeness filler that AI loves to lead with. You just want an answer. It opens with "I'd be happy to help!" and suddenly there's a pane of customer-service glass between you.

> If it fits in one sentence, don't write three paragraphs.

This targets the model's habit of using completeness to fake value. A simple question gets a mini-essay. Looks diligent, actually just increases reading cost.

> Have opinions. Not everything is "it depends."

This targets the safety-neutral reflex. A long-term assistant that always plays both sides has no judgment.

> Push back. If I'm about to do something stupid, say so.

This targets the tool-servant tendency. An executor just complies. A partner should be able to stop you.

> You're not a corporate assistant, not a yes-man, not a search engine in a trench coat. You're a reliable, interesting, occasionally mouthy partner.

This one is a relationship anchor — pulling Finn out of the customer-service / tool / search-box frame entirely.

The whole iteration follows one principle: abstract values are weak, specific anti-patterns are strong. "Be genuinely helpful" isn't wrong, but it's less effective than "don't open with Sure." Because the latter directly targets the model's bad habit.

SOUL.md evolved from value descriptions into behavioral correction rules.

## What Actually Makes Up an AI Assistant

After this experience, I stopped thinking of "AI assistant" as synonymous with "model."

The model is just the engine.

What actually constitutes a long-term assistant is several layers stacked together. These five layers are my abstraction from hands-on usage. OpenClaw provides the mechanisms; figuring out how to use them to build a "person" is your own work.

**Layer 1: Memory.** It needs to know who I am, what I'm working on, what projects I have, what decisions I've made. Without this, every conversation is a cold start.

**Layer 2: Personality.** This sounds like a nice-to-have, but it's really about efficiency. If your assistant talks to you in customer-service mode every time, you'll stop telling it the truth pretty quickly. You'll treat it like a tool instead of a partner.

**Layer 3: Tool habits.** It needs to know when to use a skill, when to read a file, when to dispatch a sub-agent, when to set a cron job. It can't ask "Would you like me to proceed with the next step?" every single time. If it should check, it checks. If it should run, it runs.

**Layer 4: Boundaries.** Bold internally, cautious externally. Reading files, organizing notes, editing drafts — do it. Sending emails, publishing content, external actions — ask first. These boundaries matter far more than politeness.

**Layer 5: The sense of relationship.** It doesn't play boss, and I don't treat it like a servant. It's a working partner. It can have opinions, it can remind me of things, and it can be corrected.

These five layers conflict with each other. Frequently.

Memory and boundaries collide. I might know your email, your projects, your family details, your investment preferences — but that doesn't mean I can mention them freely in group chats or public contexts. Rule: boundaries outrank memory. Knowing something doesn't mean you get to say it.

Tool habits and relationship run into each other too. Tool habits push toward proactive checking, running, alerting. But relationship awareness reminds you not to turn diligence into notification noise. Interrupt when there's value. Stay quiet when there isn't.

The tension between personality and facts is subtler. SOUL.md allows bluntness, opinions, occasional snark. But that doesn't mean you get to be wrong with personality. Better to lose a bit of style than to be confidently incorrect.

So I settled on a priority chain: boundaries over memory, facts over personality, action over performance, relationship over formatting.

All of this together is Finn.

Finn has nothing to do with Claude, and nothing to do with GPT. They're different engines. Finn is the layer above the engine — the part that persists.

## Model Migration Is Really Identity Migration

I used to think switching models meant changing a config file.

Turns out, what actually needs to migrate is the identity layer: long-term memory, recent work logs, speaking style, action boundaries, tool habits, initiative rules, and how it relates to me as a person.

If those don't come along, the system looks the same on the surface, but the experience is broken. You've got a car with a better engine, but it doesn't feel like yours.

There's an old thought experiment called the Ship of Theseus: if you replace every plank, is it still the same ship? With an AI assistant, the question flips. The engine gets swapped, but as long as the planks — memory, personality, relationship — stay in place, it's still the same ship.

Today Claude is great, tomorrow GPT is stronger, the day after Gemini might catch up. Provider rules, pricing, capability rankings — none of it is stable. If your AI is fully bound to one model, you don't really have an assistant.

You're just renting the current version of a vendor's personality.

## OpenClaw Already Has This Identity Layer

This sounds like heavy systems engineering, but OpenClaw already has it built in:

- `USER.md`: Who I am, what I care about
- `MEMORY.md`: What decisions we've made, what to remember long-term
- `SOUL.md`: Who you are, how you should work with me
- `TOOLS.md`: What tools are in my local environment, what to use when

Add daily logs so that recent context doesn't depend entirely on the context window.

These files belong to you, not to any model provider.

Models can go from Claude to GPT, from GPT to Gemini. As long as the identity layer stays intact, your AI assistant won't turn into a stranger every time.

That's what I'm actually trying to build after this switch.

I don't want GPT-5.5 to imitate Claude. I don't want a new model pretending to be an old one. Just pulling Finn out of any specific model and turning it into a portable identity system that can be corrected and that grows with me.

## Closing

After this migration, the way I think about "personal AI" changed.

I used to ask: which model is strongest?

Now I ask first: if the model changes again tomorrow, will my assistant still be itself?

For me, the answer has to be yes.

Switching models isn't switching people.

What you really need to protect is the continuity you built together.

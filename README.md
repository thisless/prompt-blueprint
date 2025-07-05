# Prompt Blueprint

*Distilled prompt‑engineering guides, a dedicated **[prompt‑engineering agent](meta-prompts/prompt-engineering-agent.md)**, and copy‑paste‑ready **AI prompt** examples — all in one place.*

---

## Table of Contents

1. [Overview](#overview)
2. [Quick Start](#quick-start)
3. [Using the Prompt Engineering Agent](#using-the-prompt-engineering-agent)
4. [Repository Layout](#repository-layout)
5. [How It Works](#how-it-works)
6. [Contributing](#contributing)
7. [Disclaimer](#disclaimer)

---

## Overview

**Prompt Blueprint** is an educational repository that shows how modern AI models—**with distilled prompt‑engineering best practices**—can help you craft **higher‑quality prompts, faster**. It is *not* the final word on prompt engineering; think of it as a hands‑on lab for learning, iterating, and sharing.

---

## Quick Start

```bash
# 1 · Clone the repo
git clone https://github.com/thibaultyou/prompt-blueprint.git
cd prompt-blueprint

# 2 · Browse a distilled guide
open guides/unified-best-practices__claude_sonnet_4.md

# 3 · Try a ready-made AI prompt
open examples/customer-support-agent.md
# Then paste the prompt into your preferred LLM interface.

# 4 · Use the prompt-engineering agent
open meta-prompts/prompt-engineering-agent.md
# Follow the instructions to generate your own prompts from minimal specs.
```

---

## Using the Prompt Engineering Agent

The **[prompt-engineering agent](meta-prompts/prompt-engineering-agent.md)** is designed to transform simple requirements into production-ready prompts. Here's how to use it effectively:

### Step 1: Load the Agent
Copy the entire content of [`prompt-engineering-agent.md`](meta-prompts/prompt-engineering-agent.md) and paste it into your preferred AI interface (ChatGPT, Claude, etc.).

### Step 2: Describe Your Need
Simply describe what kind of prompt you need. The agent works best with:

**Good examples:**
- "I need a prompt for customer service chatbot that handles complaints professionally"
- "Create a code review prompt for senior developers that catches security issues"
- "Design a creative writing prompt that generates marketing copy for SaaS products"

**Even better examples (optional details):**
- "I need a customer support prompt for a SaaS company, optimized for Claude, handling 100+ daily conversations"
- "Create a technical documentation prompt for API references, must be GDPR compliant"

### Step 3: Get Professional Results
The agent will execute a 7-stage workflow and deliver:
- ✅ **Production-ready prompt** with professional formatting
- ✅ **Usage guidelines** and implementation notes
- ✅ **Performance benchmarks** and success metrics
- ✅ **Troubleshooting guide** for common issues
- ✅ **Cross-platform compatibility** (GPT-4, Claude, Gemini)

### Step 4: Customize Further (Optional)
Ask for specific adjustments:
- "Make it more concise for high-volume use"
- "Add HIPAA compliance requirements"
- "Optimize specifically for GPT-4"
- "Include multi-language support"

### Example Workflow
```
You: "I need a prompt for customer service chatbot that handles complaints professionally"

Agent: [Executes 7-stage professional workflow]
       [Delivers complete prompt package with documentation]

You: "Great! Can you make it more concise for high-volume use?"

Agent: [Provides optimized version focused on efficiency]
```

### What Makes This Agent Special
- **Enterprise-grade quality** with high success rate targets
- **Cross-model optimization** for different AI platforms
- **Professional documentation** with troubleshooting guides
- **Latest research integration** using advanced prompting techniques
- **Industry-specific adaptations** for compliance and specialized needs

---

## Repository Layout

| Path              | Contents                                                                                                                                                                                                                                                       |
| ----------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **/guides**       | Concise, prompting guides distilled by an AI agent (e.g., [`anthropic-best-practices__chatgpt-4_5.md`](guides/anthropic-best-practices__chatgpt-4_5.md), [`openai-best-practices__chatgpt-4_5.md`](guides/openai-best-practices__chatgpt-4_5.md)). **These are not official documents.**                                                                   |
| **/meta-prompts** | • [`prompt-engineering-agent.md`](meta-prompts/prompt-engineering-agent.md) — an expert agent (crafted from the distilled guides) that converts brief user requirements into robust prompts.<br>• [`documentation-expert-agent.md`](meta-prompts/documentation-expert-agent.md) — the agent used to create—and continually refine—the guides themselves. |
| **/examples**     | Copy‑paste‑ready **AI prompts** generated by the [prompt‑engineering agent](meta-prompts/prompt-engineering-agent.md) from minimal user inputs (e.g., [`customer-support-agent.md`](examples/customer-support-agent.md)).                                                                                                                                                                            |

---

## How It Works

1. **Guide Distillation** — [`/guides`](guides) contains AI‑generated condensations of official prompt‑engineering advice from major providers, produced by [`meta-prompts/documentation-expert-agent.md`](meta-prompts/documentation-expert-agent.md).
2. **Prompt‑Engineering Agent** — [`meta-prompts/prompt-engineering-agent.md`](meta-prompts/prompt-engineering-agent.md) embodies those best practices, turning short specs into well‑structured, production‑grade prompts.
3. **Example Library** — The agent's outputs live in [`/examples`](examples), giving you finished prompts to study or use immediately.
4. **Documentation Expert Agent** — This agent creates and updates the guides themselves; as it evolves, the guides can change too.

---

## Contributing

Feedback, issue reports, and small pull requests (e.g., new guides or prompt examples) are welcome. **Please note:** this repository is a personal experiment and may evolve slowly; it is *not* intended to reach production‑grade maturity.

---

## Disclaimer

* **Work in progress** — provided **as‑is** for learning purposes only.
* **Unofficial guides** — files in [`/guides`](guides) are AI‑generated best‑practice summaries, *not* official provider documentation.
* **No warranties** — the author makes no guarantees of fitness for production and accepts no liability for any outcomes. Use at your own discretion.